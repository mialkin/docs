# Statistics

## Содержание

- [Statistics](#statistics)
  - [Содержание](#содержание)
  - [Число строк](#число-строк)
  - [Доля неопределенных значений](#доля-неопределенных-значений)
  - [Наиболее частые значения](#наиболее-частые-значения)

## Число строк

Начнем с оценки кардинальности в простом случае запроса без предикатов.

```sql
EXPLAIN
SELECT *
FROM flights;
```

```console
Seq Scan on flights  (cost=0.00..4772.67 rows=214867 width=63)
```

Точное значение:

```sql
SELECT COUNT(*)
FROM flights;
```

```console
SELECT COUNT(*)
FROM flights;
-- 214867
```

Оптимизатор получает значение из таблицы `pg_class`:

```sql
SELECT reltuples, relpages
FROM pg_class
WHERE relname = 'flights';
```

| reltuples | relpages |
| :-------- | :------- |
| 214867    | 2624     |

Значение параметра, управляющего ориентиром статистики, по умолчанию равно `100`:

```sql
SHOW default_statistics_target;
```

| default_statistics_target |
| :------------------------ |
| 100                       |

Поскольку при анализе таблицы учитывается `300` $\times$ `default_statistics_target` строк, то оценки для относительно крупных таблиц могут не быть абсолютно.

## Доля неопределенных значений

Часть рейсов еще не отправились, поэтому время вылета для них не определено:

```sql
EXPLAIN
SELECT *
FROM flights
WHERE actual_departure IS NULL;
```

```console
Seq Scan on flights  (cost=0.00..4772.67 rows=16115 width=63)
  Filter: (actual_departure IS NULL)
```

Точное значение:

```sql
SELECT COUNT(*)
FROM flights
WHERE actual_departure IS NULL;
-- 16348
```

Оценка оптимизатора получена как общее число строк, умноженное на долю `NULL`-значений :

```sql
SELECT 214867 * null_frac
FROM pg_stats
WHERE tablename = 'flights'
  AND attname = 'actual_departure';
-- 16115.02564035356
```

## Наиболее частые значения

Для эксперимента ограничим размер списка наиболее частых значений (который по умолчанию определяется параметром `default_statistics_target`) на уровне столбца:

```sql
ALTER TABLE flights
    ALTER COLUMN arrival_airport
        SET STATISTICS 10;
```

```sql
ANALYZE flights;
```

Если значение попало в список наиболее частых, селективность можно узнать непосредственно из статистики. Пример (Шереметьево):

```sql
EXPLAIN
SELECT *
FROM flights
WHERE arrival_airport = 'SVO';
```

```console
Seq Scan on flights  (cost=0.00..5309.84 rows=19617 width=63)
  Filter: (arrival_airport = 'SVO'::bpchar)
```

Точное значение:

```sql
SELECT COUNT(*)
FROM flights
WHERE arrival_airport = 'SVO';
-- 19348
```

Вот как выглядит список наиболее частых значений и частота их встречаемости:

```sql
SELECT most_common_vals, most_common_freqs
FROM pg_stats
WHERE tablename = 'flights'
  AND attname = 'arrival_airport'
```

| most_common_vals                          | most_common_freqs                                                                                                                                                                                               |
| :---------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| {DME,SVO,LED,VKO,OVB,KJA,SVX,ROV,PEE,AER} | {0.09683333337306976,0.09130000323057175,0.055166665464639664,0.05303333327174187,0.030233332887291908,0.022600000724196434,0.021299999207258224,0.018666666001081467,0.018533334136009216,0.01850000023841858} |

Кардинальность вычисляется как число строк, умноженное на частоту значения:

```sql
SELECT 214867 * s.most_common_freqs[array_position((s.most_common_vals::text::text[]), 'SVO')]
FROM pg_stats s
WHERE s.tablename = 'flights'
  AND s.attname = 'arrival_airport';
-- 19617.35779414326
```

`19617` — это то же значение, что было получено для `SVO` в плане выше.

Список наиболее частых значений может использоваться и для оценки селективности неравенств. Для этого в `most_common_vals` надо найти все значения, удовлетворяющие неравенству, и просуммировать частоты соответствующих элементов из `most_common_freqs`.
