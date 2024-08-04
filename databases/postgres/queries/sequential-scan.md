# Sequential access

- Sequential scan
- Parallel execution plans
- Parallel sequential scan
- Aggregation during parallel execution
- `EXPLAIN` command

## Table of contents

- [Sequential access](#sequential-access)
  - [Table of contents](#table-of-contents)
  - [Sequential scan](#sequential-scan)
  - [Aggregation](#aggregation)
  - [Parallel sequential scan](#parallel-sequential-scan)
    - [Parallel Seq Scan](#parallel-seq-scan)
    - [Partial Aggregate](#partial-aggregate)
    - [Gather](#gather)
    - [Finalize Aggregate](#finalize-aggregate)

## Sequential scan

В плане выполнения запроса последовательное сканирование представлено узлом `Seq Scan`:

```sql
EXPLAIN
SELECT *
FROM flights;
```

```console
Seq Scan on flights  (cost=0.00..4772.67 rows=214867 width=63)
```

В скобках приведены важные значения:

- cost — оценка стоимости;
- rows — оценка числа строк, возвращаемых операцией;
- width — оценка размера одной записи в байтах.

Стоимость указывается в некоторых условных единицах и состоит из двух компонент.

Первое число показывает начальную стоимость вычисления узла. Для последовательного сканирования это ноль — чтобы начать возвращать данные, никакой подготовки не требуется.

Второе число показывает полную стоимость для получения всех данных. Как оно получается?

Оптимизатор PostgreSQL учитывает дисковый ввод-вывод и ресурсы процессора.
Составляющая ввода-вывода рассчитывается как произведение числа страниц в таблице на условную стоимость чтения одной

```sql
SELECT relpages,
       CURRENT_SETTING('seq_page_cost'),
       relpages * CURRENT_SETTING('seq_page_cost')::real AS total
FROM pg_class
WHERE relname = 'flights';
```

| relpages | current_setting | total |
| :------- | :-------------- | :---- |
| 2624     | 1               | 2624  |

Составляющая ресурсов процессора складывается из стоимости обработки каждой строки:

```sql
SELECT reltuples,
       CURRENT_SETTING('cpu_tuple_cost'),
       reltuples * CURRENT_SETTING('cpu_tuple_cost')::real AS total
FROM pg_class
WHERE relname = 'flights';
```

| reltuples | current_setting | total   |
| :-------- | :-------------- | :------ |
| 214867    | 0.01            | 2148.67 |

## Aggregation

```sql
EXPLAIN
SELECT COUNT(*)
FROM seats;
```

```console
Aggregate  (cost=24.74..24.75 rows=1 width=8)
   ->  Seq Scan on seats  (cost=0.00..21.39 rows=1339 width=0)
```

План состоит из двух узлов. Верхний — Aggregate, в котором происходит вычисление count, — получает данные от нижнего — Seq Scan.
Обратите внимание на стоимость узла Aggregate: начальная стоимость практически равна полной. Это означает, что узел не может выдать результат, пока не обработает все данные (что вполне логично).

Разница между оценкой для Aggregate и верхней оценкой для Seq Scan — стоимость работы собственно узла Aggregate. Она вычисляется исходя из оценки ресурсов на выполнение элементарной операции:

```sql
SELECT reltuples,
       CURRENT_SETTING('cpu_operator_cost'),
       reltuples * CURRENT_SETTING('cpu_operator_cost')::real AS total
FROM pg_class
WHERE relname = 'seats';
```

| reltuples | current_setting | total     |
| :-------- | :-------------- | :-------- |
| 1339      | 0.0025          | 3.3474998 |

## Parallel sequential scan

Рассмотрим пример плана с параллельным последовательным сканированием:

```sql
EXPLAIN
SELECT COUNT(*)
FROM bookings;
```

```console
Finalize Aggregate  (cost=25483.58..25483.59 rows=1 width=8)
  ->  Gather  (cost=25483.36..25483.57 rows=2 width=8)
        Workers Planned: 2
        ->  Partial Aggregate  (cost=24483.36..24483.37 rows=1 width=8)
              ->  Parallel Seq Scan on bookings  (cost=0.00..22284.29 rows=879629 width=0)
```

Все, что находится ниже узла Gather — параллельная часть плана. Она выполняется в каждом из рабочих процессов (которых запланировано два) и, возможно, в ведущем процессе.

Узел Gather и все узлы выше выполняются только в ведущем процессе. Это последовательная часть плана.

### Parallel Seq Scan

Начнем разбираться снизу вверх. Узел Parallel Seq Scan представляет сканирование таблицы в параллельном режиме.
В поле rows показана оценка числа строк, которые обработает один рабочий процесс. Всего их запланировано 2, и еще часть работы выполнит ведущий, поэтому общее число строк делится на 2.4 (доля ведущего процесса уменьшается с ростом числа рабочих процессов).

```sql
SELECT ROUND(reltuples / 2.4) "rows"
FROM pg_class
WHERE relname = 'bookings';
```

```console
879629
```

```sql
SELECT COUNT(*)
FROM bookings;
```

```console
2111110
```

$879629 \times 2.4 = 2111110$

Для оценки узла Parallel Seq Scan компонента ввода-вывода берется полностью (таблицу все равно придется прочитать страница за страницей), а ресурсы процессора делятся между процессами (на 2.4 в данном случае).

```sql
SELECT ROUND(
               (
                   relpages * CURRENT_SETTING('seq_page_cost')::real +
                   reltuples * CURRENT_SETTING('cpu_tuple_cost')::real / 2.4
                   ):: numeric,
               2) "cost"
FROM pg_class
WHERE relname = 'bookings';

-- 22284.29
```

По умолчанию ведущий процесс участвует в выполнении параллельной части плана:

```sql
SHOW parallel_leader_participation;
-- on
```

Если ведущий процесс становится узким местом, его можно разгрузить:

```sql
SET parallel_leader_participation = off;
```

```sql
EXPLAIN
SELECT COUNT(*)
FROM bookings;
```

```console
Finalize Aggregate  (cost=27682.65..27682.66 rows=1 width=8)
  ->  Gather  (cost=27682.44..27682.65 rows=2 width=8)
        Workers Planned: 2
        ->  Partial Aggregate  (cost=26682.44..26682.45 rows=1 width=8)
              ->  Parallel Seq Scan on bookings  (cost=0.00..24043.55 rows=1055555 width=0)
```

В этом случае общее число строк делится на число запланированных рабочих процессов (2):

```sql
SELECT ROUND(reltuples / 2) "rows"
FROM pg_class
WHERE relname = 'bookings';

-- 1055555
```

$1055555 \times 2 = 2111110$

### Partial Aggregate

```sql
EXPLAIN
SELECT COUNT(*)
FROM bookings;
```

```console
Finalize Aggregate  (cost=25483.58..25483.59 rows=1 width=8)
  ->  Gather  (cost=25483.36..25483.57 rows=2 width=8)
        Workers Planned: 2
        ->  Partial Aggregate  (cost=24483.36..24483.37 rows=1 width=8)
              ->  Parallel Seq Scan on bookings  (cost=0.00..22284.29 rows=879629 width=0)
```

Следующий узел — Partial Aggregate — выполняет агрегацию данных, полученных рабочим процессом, то есть в данном случае подсчитывает количество строк. Оценка выполняется уже известным образом (и добавляется к оценке сканирования таблицы):

```sql
SELECT ROUND((reltuples / 2.4 * CURRENT_SETTING('cpu_operator_cost'):: real):: numeric, 2) "cost"
FROM pg_class
WHERE relname = 'bookings';

-- 2199.07
```

### Gather

Следующий узел — Gather — выполняется ведущим процессом. Он отвечает за запуск рабочих процессов и получение от них данных.

Запуск процессов и пересылка каждой строки данных оцениваются следующими значениями:

```sql
SELECT CURRENT_SETTING('parallel_setup_cost') parallel_setup_cost,
       CURRENT_SETTING('parallel_tuple_cost') parallel_tuple_cost;
```

| parallel_setup_cost | parallel_tuple_cost |
| :------------------ | :------------------ |
| 1000                | 0.1                 |

`parallel_setup_cost` — стоимость запуска параллельной инфраструктуры.

`parallel_tuple_cost` — стоимость передачи одной строчки между процессами.

В данном случае пересылается всего одна строки и основная стоимость приходится на запуск.

### Finalize Aggregate

```sql
EXPLAIN
SELECT COUNT(*)
FROM bookings;
```

```console
Finalize Aggregate  (cost=25483.58..25483.59 rows=1 width=8)
  ->  Gather  (cost=25483.36..25483.57 rows=2 width=8)
        Workers Planned: 2
        ->  Partial Aggregate  (cost=24483.36..24483.37 rows=1 width=8)
              ->  Parallel Seq Scan on bookings  (cost=0.00..22284.29 rows=879629 width=0)
```

Последний узел — Finalize Aggregate — агрегирует полученные частичные агрегаты. Поскольку для этого надо сложить всего три числа, оценка минимальна.
