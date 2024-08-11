# Statistics

## Содержание

- [Statistics](#statistics)
  - [Содержание](#содержание)
  - [Число строк](#число-строк)
  - [Доля неопределенных значений](#доля-неопределенных-значений)
  - [Наиболее частые значения](#наиболее-частые-значения)
  - [Число уникальных значений](#число-уникальных-значений)
  - [Частные и общие планы выполнения](#частные-и-общие-планы-выполнения)
  - [Гистограмма](#гистограмма)
  - [Дополнительные поля](#дополнительные-поля)

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

## Число уникальных значений

Если же указанного значения нет в списке наиболее частых, то оно вычисляется исходя из предположения, что все данные (кроме наиболее частых) распределены равномерно.

Например, в списке частых значений нет Владивостока.

```sql
EXPLAIN
SELECT *
FROM flights
WHERE arrival_airport = 'VVO';
```

```console
Seq Scan on flights  (cost=0.00..5309.84 rows=1312 width=63)
  Filter: (arrival_airport = 'VVO'::bpchar)
```

Точное значение:

```sql
SELECT COUNT(*)
FROM flights
WHERE arrival_airport = 'VVO';
-- 1188
```

Для получения оценки вычислим сумму частот наиболее частых значений:

```sql
SELECT SUM(f)
FROM pg_stats s,
     UNNEST(s.most_common_freqs) f
WHERE s.tablename = 'flights'
  AND s.attname = 'arrival_airport';
-- 0.42616662
```

На менее частые значения приходятся оставшиеся строки. Поскольку мы исходим из предположения о равномерности распределения менее частых значений, селективность будет равна `1`/`nd`, где `nd` — число уникальных значений:

```sql
SELECT n_distinct
FROM pg_stats s
WHERE s.tablename = 'flights'
  AND S.attname = 'arrival_airport';
-- 104
```

Учитывая, что из этих значений 10 входят в список наиболее частых, и нет неопределенных значений, получаем следующую оценку:

```sql
SELECT 214867 * (1 - 0.42616662) / (104 - 10);
-- 1311.6793283027659574
```

`1312` — это то значение, которое изначально рассчитал планировщик в запросе `EXPLAIN SELECT * FROM flights WHERE arrival_airport = 'VVO';` выше.

## Частные и общие планы выполнения

Неравномерные распределения значений приводят к тому, что запросы, отличающиеся константами или значениями параметров, могут иметь разные планы выполнения. Например, подготовим следующий запрос:

```sql
PREPARE f(text) AS SELECT *
                   FROM flights
                   WHERE status = $1;
```

Поиск отмененных рейсов будет использовать индекс, поскольку статистика говорит о том, что таких рейсов мало:

```sql
CREATE INDEX ON flights (status);
```

```console
Index Scan using flights_status_idx on flights  (cost=0.29..444.63 rows=437 width=63)
  Index Cond: ((status)::text = 'Cancelled'::text)
```

А поиск прибывших рейсов — нет, поскольку их много:

```sql
EXPLAIN EXECUTE f('Arrived');
```

```console
Seq Scan on flights  (cost=0.00..5309.84 rows=198559 width=63)
  Filter: ((status)::text = 'Arrived'::text)
```

Пять первых планирований всегда используют частные планы. Затем может оказаться, что стоимость общего плана (построенного без учета конкретного значения, в предположении равномерного распределения) не превышает среднюю стоимость уже построенных частных планов. Тогда планировщик запомнит общий план и будет использовать его, не выполняя планирование каждый раз.

Построим план еще несколько раз:

```sql
EXPLAIN EXECUTE f('Arrived');
```

На пятый раз планировщик переключится на общий план. Вместо конкретного значения в плане будет указан номер параметра:

```console
Bitmap Heap Scan on flights  (cost=401.83..3473.47 rows=35811 width=63)
  Recheck Cond: ((status)::text = $1)
  ->  Bitmap Index Scan on flights_status_idx  (cost=0.00..392.88 rows=35811 width=0)
        Index Cond: ((status)::text = $1)
```

При неравномерном распределении это может вызывать проблемы. Параметр `plan_cache_mode` позволяет отключить использование частных планов (или наоборот, с самого начала использовать общий план):

```sql
SHOW plan_cache_mode;
-- auto
```

```sql
SET plan_cache_mode = 'force_custom_plan';
```

Повторим запрос:

```sql
EXPLAIN EXECUTE f('Arrived');
```

Увидим, что снова в плане запроса появляется значение `Arrived`, то есть используется частный план:

```console
Seq Scan on flights  (cost=0.00..5309.84 rows=198559 width=63)
  Filter: ((status)::text = 'Arrived'::text)
```

```sql
RESET plan_cache_mode;
```

## Гистограмма

При условиях «больше» и «меньше» для оценки будет использоваться список наиболее частых значений, или гистограмма, или оба способа вместе. Гистограмма строится так, чтобы не включать наиболее частые значения и `NULL`:

```sql
SELECT histogram_bounds
FROM pg_stats s
WHERE s.tablename = 'flights'
  AND s.attname = 'arrival_airport';
```

| histogram_bounds                              |
| :-------------------------------------------- |
| {AAQ,CEK,GOJ,KGP,KZN,NBC,OGZ,REN,TJM,ULY,YKS} |

Число корзин гистограммы определяется параметром `default_statistics_target`, a границы выбираются так, чтобы в каждой корзине находилось примерно одинаковое количество значений.

Рассмотрим пример:

```sql
EXPLAIN
SELECT *
FROM flights
WHERE arrival_airport <= 'GOJ';
```

```console
Seq Scan on flights  (cost=0.00..5309.84 rows=49441 width=63)
  Filter: (arrival_airport <= 'GOJ'::bpchar)
```

Точное значение:

```sql
SELECT COUNT(*)
FROM flights
WHERE arrival_airport <= 'GOJ';
-- 50520
```

Как получена оценка?

Учтем частоту наиболее частых значений, попадающих в указанный интервал:

```sql
SELECT SUM(s.most_common_freqs[array_position((s.most_common_vals::text::text[]), v)])
FROM pg_stats s,
     UNNEST(s.most_common_vals::text::text[]) v
WHERE s.tablename = 'flights'
  AND s.attname = 'arrival_airport'
  AND v <= 'GOJ';
-- 0.11533333
```

Указанный интервал занимает ровно 2 корзины гистограммы из 10, а неопределенных значений в данном столбце нет, получаем следующую оценку:

```sql
SELECT 214867 * (1 - 0.42616662) * (2.0 / 10.0) + 214867 * 0.11533333;
-- 49440.897989202
```

`49441` — то значение, которое выше получено планировщиком при оценке стоимости запроса `EXPLAIN SELECT * FROM flights WHERE arrival_airport <= 'GOJ';`.

В общем случае учитываются и не полностью занятые корзины (с помощью линейной аппроксимации).

## Дополнительные поля

- Упорядоченность (использовать ли битовую карту?)
  - `pg_stats.correlation` (`1` — по возрастанию, `0` — хаотично, `-1` — по убыванию)
- Видимость (использовать ли сканирование только индекса?)
  - `pg_class.relallvisible`
- Средний размер значения в байтах (оценка памяти)
  - `pg_stats.avg_width`
- Информация об элементах массивов, tsvector и т. п.
  - `pg_stats.most_common_elems`
  - `pg_stats.most_common_elem_freqs`
  - `pg_stats.elem_count_histogram`
