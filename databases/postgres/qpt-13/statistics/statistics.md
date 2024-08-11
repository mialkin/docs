# Statistics

## Содержание

- [Statistics](#statistics)
  - [Содержание](#содержание)
  - [Число строк](#число-строк)
  - [Доля неопределенных значений](#доля-неопределенных-значений)

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
