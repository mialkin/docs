# Merge joins and sorting

## Содержание

- [Merge joins and sorting](#merge-joins-and-sorting)
  - [Содержание](#содержание)
  - [Соединение слиянием](#соединение-слиянием)
    - [Вычислительная сложность](#вычислительная-сложность)
    - [Параллельный режим](#параллельный-режим)
  - [Сортировка](#сортировка)
    - [Сортировка в памяти](#сортировка-в-памяти)
    - [Группировка и уникальные значения](#группировка-и-уникальные-значения)
    - [Внешняя сортировка](#внешняя-сортировка)
    - [Параллельная сортировка](#параллельная-сортировка)

## Соединение слиянием

Для данного метода соединения важно то, что в качестве предварительной подготовки нам нужно иметь два отсортированных набора строк. В данном случае оба набора строк отсортированы по условию соединения.

```sql
SELECT a.title, s.name
FROM albums a
         JOIN songs s ON a.id = s.a_id;
```

<img src="merge_joins_1.jpeg" width="600px" alt="Соединение слиянием"/>

Сортировка может выполнять разными вариантами:

- сортировка данных в чистом виде
- получение отсортированных данных от нижестоящего узла плана
- использовать индекс по условию соединения

<img src="merge_joins_2.jpeg" width="600px" alt="Принцип работы 2"/>

Упрощенно говоря, алгоритм такой: когда идентификатор с одной стороны больше, чем с другой, то сдвигаем строчку вниз в том наборе, где идентификатор меньше.

<img src="merge_joins_3.jpeg" width="600px" alt="Принцип работы 3"/>

<img src="merge_joins_4.jpeg" width="600px" alt="Принцип работы 4"/>

Если результат необходим в отсортированном виде, оптимизатор может предпочесть соединение слиянием. Особенно, если данные от дочерних узлов можно получить уже отсортированными — как в этом примере:

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM tickets t
         JOIN ticket_flights tf ON tf.ticket_no = t.ticket_no
ORDER BY t.ticket_no;
```

```console
Merge Join
  Merge Cond: (t.ticket_no = tf.ticket_no)
  ->  Index Scan using tickets_pkey on tickets t
  ->  Index Scan using ticket_flights_pkey on ticket_flights tf
```

Вот еще один пример с двумя соединениями слиянием, в котором один узел `Merge Join` получает отсортированный набор от другого узла `Merge Join`:

```sql
EXPLAIN (COSTS OFF)
SELECT t.ticket_no, bp.flight_id, bp.seat_no
FROM tickets t
         JOIN ticket_flights tf ON t.ticket_no = tf.ticket_no
         JOIN boarding_passes bp ON bp.ticket_no = tf.ticket_no
    AND bp.flight_id = tf.flight_id
ORDER BY t.ticket_no;

```

```console
Merge Join
  Merge Cond: (tf.ticket_no = t.ticket_no)
  ->  Merge Join
        Merge Cond: ((tf.ticket_no = bp.ticket_no) AND (tf.flight_id = bp.flight_id))
        ->  Index Only Scan using ticket_flights_pkey on ticket_flights tf
        ->  Index Scan using boarding_passes_pkey on boarding_passes bp
  ->  Index Only Scan using tickets_pkey on tickets t
```

Здесь соединяются перелеты (`ticket_flights`) и посадочные талоны (`boarding_passes`), и с этим, уже отсортированным по номерам билетов, набором строк соединяются билеты (`tickets`).

### Вычислительная сложность

Вычислительная сложность приблизительно равна:

- $N + M$, если не требуется сортировка
- $N \times log_{2}{N} + M \times log_{2}{M}$, если сортировка нужна

$N$ и $M$ — число строк в первом и втором наборах данных.

Возможны начальные затраты на сортировку. Данный тип соединения эффективен для большого количества строк. Используется как в OLAP, так и в OLTP приложениях.

Посмотрим на стоимость соединения слиянием:

```sql
EXPLAIN
SELECT *
FROM tickets t
         JOIN ticket_flights tf ON tf.ticket_no = t.ticket_no
ORDER BY t.ticket_no;
```

```console
Merge Join  (cost=0.99..822484.28 rows=8391852 width=136)
  Merge Cond: (t.ticket_no = tf.ticket_no)
  ->  Index Scan using tickets_pkey on tickets t  (cost=0.43..139135.32 rows=2949857 width=104)
  ->  Index Scan using ticket_flights_pkey on ticket_flights tf  (cost=0.56..571076.24 rows=8391852 width=32)
```

Начальная стоимость включает:

- сумму начальных стоимостей дочерних узлов (включает стоимость сортировки, если она необходима)
- стоимость получения первой пары строк, соответствующих друг другу

В отличие от соединения хешированием, слияние без сортировки хорошо подходит для случая, когда надо быстро получить первые строки.

```sql
EXPLAIN
SELECT *
FROM tickets t
         JOIN ticket_flights tf ON tf.ticket_no = t.ticket_no
ORDER BY t.ticket_no
LIMIT 1000;
```

```console
Limit  (cost=0.99..99.00 rows=1000 width=136)
  ->  Merge Join  (cost=0.99..822484.28 rows=8391852 width=136)
        Merge Cond: (t.ticket_no = tf.ticket_no)
        ->  Index Scan using tickets_pkey on tickets t  (cost=0.43..139135.32 rows=2949857 width=104)
        ->  Index Scan using ticket_flights_pkey on ticket_flights tf  (cost=0.56..571076.24 rows=8391852 width=32)
```

Обратите внимание и на то, как уменьшилась общая стоимость.

### Параллельный режим

Соединение слиянием может использоваться и в параллельных планах.

Первый, внешний, набор данных сканируется параллельно несколькими процессами. Дальше каждый из этих процессов последовательно сканирует второй набор данных.

<img src="parallel_mode.jpeg" width="800px" alt="Параллельный режим"/>

## Сортировка

### Сортировка в памяти

Если оперативной памяти хватает, то выполняется **быстрая сортировка** (quick sort).

Также есть метод частичной **пирамидальной сортировки** (top-N heapsort). Он используется, когда нужна только часть значений, а не весь отсортированный набор (например, `LIMIT N`).

Есть также **инкрементальная сортировка**. Применяется когда данные уже отсортированы, но не по всем ключам. Например, нам нужно отсортировать данные по трем столбцам, и у нас есть индекс по первым двум столбцам, а по третьему нет. В этом случае мы берем уже отсортированные данные по первым двум столбцам, и дополняем их данными по третьему столбцу. Нам, по-сути дела, нужно не полностью весь набор пересортировать, а только досортировать третий столбец в рамках групп по первым двум столбцам, которые уже отсортированы.

Сортировка выполняется в узле `Sort`. Чтобы начать выдавать данные вышестоящему узлу, сортировка должна быть полностью завершена.

Вот пример плана с соединением слиянием, использующим сортировку (здесь из-за небольшого размера таблицы `aircraft_code` вместо чтения индекса выбрана явная сортировка):

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM aircrafts a
         JOIN seats s ON a.aircraft_code = s.aircraft_code
ORDER BY a.aircraft_code;
```

```console
Merge Join
  Merge Cond: (s.aircraft_code = ml.aircraft_code)
  ->  Index Scan using seats_pkey on seats s
  ->  Sort
        Sort Key: ml.aircraft_code
        ->  Seq Scan on aircrafts_data ml
```

В следующем примере для сортировки используется быстрая сортировка (`Sort Method`). В той же строке указан использованный объем памяти:

```sql
EXPLAIN (ANALYZE, TIMING OFF, SUMMARY OFF)
SELECT *
FROM seats
ORDER BY seat_no;
```

```console
Sort  (cost=90.93..94.28 rows=1339 width=15) (actual rows=1339 loops=1)
  Sort Key: seat_no
  Sort Method: quicksort  Memory: 101kB
  ->  Seq Scan on seats  (cost=0.00..21.39 rows=1339 width=15) (actual rows=1339 loops=1)
```

Если набор строк ограничен, планировщик может переключиться на частичную сортировку (грубо говоря, вместо полной сортировки здесь 100 раз находится максимальное значение):

```sql
EXPLAIN (ANALYZE, TIMING OFF, SUMMARY OFF)
SELECT *
FROM seats
ORDER BY seat_no
LIMIT 100;
```

```console
Limit  (cost=72.57..72.82 rows=100 width=15) (actual rows=100 loops=1)
  ->  Sort  (cost=72.57..75.91 rows=1339 width=15) (actual rows=100 loops=1)
        Sort Key: seat_no
        Sort Method: top-N heapsort  Memory: 32kB
        ->  Seq Scan on seats  (cost=0.00..21.39 rows=1339 width=15) (actual rows=1339 loops=1)
```

Обратите внимание, что стоимость запроса снизилась, и для сортировки потребовалось меньше памяти.

Пример инкрементальной сортировки, доступной с 13-й версии PostgreSQL:

```sql
EXPLAIN (ANALYZE, COSTS OFF, TIMING OFF, SUMMARY OFF)
SELECT *
FROM tickets
ORDER BY ticket_no, passenger_id;
```

```console
Incremental Sort (actual rows=2949857 loops=1)
  Sort Key: ticket_no, passenger_id
  Presorted Key: ticket_no
  Full-sort Groups: 92184  Sort Method: quicksort  Average Memory: 28kB  Peak Memory: 28kB
  ->  Index Scan using tickets_pkey on tickets (actual rows=2949857 loops=1)
```

Здесь данные, полученные из таблицы `tickets` по индексу `tickets_pkey`, уже отсортированы по столбцу `ticket_no` (`Presorted Key`), поэтому остается доупорядочить строки по столбцу `passenger_id`. Для сортировки отдельных групп использовалась быстрая сортировка.

### Группировка и уникальные значения

Как мы [видели](../hash-joins/hash-joins.md#группировка-и-уникальные-значения) в предыдущей теме, для устранения дубликатов может использоваться хеширование. Другой способ — сортировка значений:

```sql
Result
  ->  Unique
        ->  Index Only Scan using ticket_flights_pkey on ticket_flights
```

Такой способ особенно выгоден, если требуется получить отсортированный результат (как в данном случае).

Устранение дубликатов происходит в узле `Unique`. Он получает отсортированный набор строк (от узла `Sort` или, как в этом примере, от индексного сканирования) и убирает повторяющиеся значения.

Для группировки с помощью `GROUP BY` будет использоваться похожий по смыслу узел `GroupAggregate`.

### Внешняя сортировка

Если оперативной памяти `work_mem` не хватает, чтобы выполнить сортировку полностью в памяти, в этом случае будут использоваться временные файлы.

Если набор строк для сортировки не помещается целиком в оперативную память размером `work_mem`, применяется внешняя сортировка с использованием временных файлов. Вот пример такого плана (`Sort Method: external merge`):

```sql
EXPLAIN (ANALYZE, BUFFERS, TIMING OFF, SUMMARY OFF)
SELECT *
FROM flights
ORDER BY scheduled_departure;
```

```console
Sort  (cost=31883.96..32421.12 rows=214867 width=63) (actual rows=214867 loops=1)
  Sort Key: scheduled_departure
  Sort Method: external merge  Disk: 17112kB
  Buffers: shared hit=3 read=2624, temp read=2139 written=2145
  ->  Seq Scan on flights  (cost=0.00..4772.67 rows=214867 width=63) (actual rows=214867 loops=1)
        Buffers: shared read=2624
Planning:
  Buffers: shared hit=64 read=13 dirtied=2
```

Обратите внимание на то, что узел `Sort` записывает и читает временные данные
(`temp read` и `written`).

Увеличим значение `work_mem`:

```sql
SET work_mem = '48MB';

EXPLAIN (ANALYZE, BUFFERS, TIMING OFF, SUMMARY OFF)
SELECT *
FROM flights
ORDER BY scheduled_departure;
```

```console
Sort  (cost=23802.46..24339.62 rows=214867 width=63) (actual rows=214867 loops=1)
  Sort Key: scheduled_departure
  Sort Method: quicksort  Memory: 26161kB
  Buffers: shared hit=2624
  ->  Seq Scan on flights  (cost=0.00..4772.67 rows=214867 width=63) (actual rows=214867 loops=1)
        Buffers: shared hit=2624
```

Теперь все строки поместились в память, и планировщик выбрал более дешевую быструю сортировку.

```sql
RESET work_mem;
```

### Параллельная сортировка
