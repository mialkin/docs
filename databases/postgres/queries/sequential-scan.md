# Sequential access

- Sequential scan
- Parallel execution plans
- Parallel sequential scan
- Aggregation during parallel execution
- `EXPLAIN` command

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
