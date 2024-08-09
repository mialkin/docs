# Hash joins

## Table of contents

- [Hash joins](#hash-joins)
  - [Table of contents](#table-of-contents)
  - [One-pass hash joins](#one-pass-hash-joins)

## One-pass hash joins

Однопроходное соединение применяется тогда, когда оперативной памяти достаточно, чтобы построить хеш-таблицу.

```sql
SELECT a.title, s.name
FROM albums
         JOIN songs s ON a.id = s.album_id;
```

Помимо идентификатора условия соединения внутри хеш-таблицы также должны находиться все столбцы, которые используются в запросе. Столбце `title` (левый рисунок), нам потребуется для результата, поэтому он точно также должен быть помещен в хеш-таблицу (правый рисунок). Столбец `year` не нужен в результатах запроса, поэтому он в хеш-таблицу помещаться не будет.

<img src="hash-table-1.jpeg" width="800px" alt="Хеш-таблица"/>

Размер хеш-таблицы вычисляется по формуле [↑ `work_mem`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-WORK-MEM) $\times$ [↑ `hash_mem_multiplier`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-HASH-MEM-MULTIPLIER).

После того, как хеш-таблица построена, мы читаем второй, внешний, набор строк (правый рисунок), и для каждой строки вычисляем хеш-код. Далее по хеш-коду идем в соответствующее ведро хеш-таблицы и проверяем условие соединения.

<img src="hash-table-2.jpeg" width="800px" alt="Хеш-таблица"/>

Для большой выборки оптимизатор предпочитает соединение хешированием:

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM tickets t
         JOIN ticket_flights tf ON tf.ticket_no = t.ticket_no;
```

```console
Hash Join
  Hash Cond: (tf.ticket_no = t.ticket_no)
  ->  Seq Scan on ticket_flights tf
  ->  Hash
        ->  Seq Scan on tickets t
```

Узел Hash Join начинает работу с того, что обращается к дочернему узлу
Hash. Тот получает от своего дочернего узла (здесь — Seq Scan) весь набор строк и строит хеш-таблицу.

Затем Hash Join обращается ко второму дочернему узлу и соединяет строки,
постепенно возвращая полученные результаты.
