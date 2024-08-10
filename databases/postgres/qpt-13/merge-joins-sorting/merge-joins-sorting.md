# Merge joins and sorting

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

## Вычислительная сложность

Вычислительная сложность приблизительно равна:

- $N + M$, если не требуется сортировка
- $N \times log_{2}{N} + M \times log_{2}{M}$, если сортировка нужна

 $N$ и $M$ — число строк в первом и втором наборах данных.

Возможны начальные затраты на сортировку. Данный тип соединения эффективен для большого количества строк. Используется как в OLAP, так и в OLTP приложениях.
