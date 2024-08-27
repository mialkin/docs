# EAV, entity–attribute–value model

The **EAV model** is a data modeling technique often used in situations where the number of attributes that can be associated with a data entity is large, sparse, or variable.

The EAV model is often used in applications like:

- Healthcare systems where patients can have a wide variety of data points, but not every patient will have every possible data point recorded
- Content management systems where different content types may have widely varying fields
- Product catalogs where different products may have a different set of attributes

Consider a database storing health information:

| Entity    | Attribute      | Value       |
| --------- | -------------- | ----------- |
| Patient A | Height         | 175 cm      |
| Patient A | Weight         | 70 kg       |
| Patient B | Blood Pressure | 120/80 mmHg |
| Patient B | Weight         | 85 kg       |

Disadvantages of the EAV model:

- Complexity: querying data can be more complex, as it often requires joining multiple tables and pivoting data
- Performance: since data is stored in a highly generic format, querying large datasets can be slow
- Validation: it can be harder to enforce data integrity rules or constraints compared to more traditional models

## Alternatives

An alternative could be JSONB in Postgres, or MongoDB, but later does not support transactions.

The EAV model is a [↑ slippery slope](https://stackoverflow.com/questions/494158/best-beginner-resources-for-understanding-the-eav-database-model). It can work fine in a small tightly-scoped database, but blows up in your face the nearer you get to a production system.

[↑ Entity-attribute-value (EAV) design in PostgreSQL - don't do it!](https://www.cybertec-postgresql.com/en/entity-attribute-value-eav-design-in-postgresql-dont-do-it/).

[↑ Bad CaRMa](https://www.red-gate.com/simple-talk/opinion/opinion-pieces/bad-carma).

[↑ EAV Zero to EAV Hero!](https://www.youtube.com/watch?v=WneHTRZVbec).

## SQL

```sql
BEGIN TRANSACTION;

CREATE TABLE products
(
    id    bigserial,
    title varchar NOT NULL
);

INSERT INTO products (id, title)
VALUES (DEFAULT, 'Adidas Originals NMD_R1 Primebleu');

INSERT INTO products (id, title)
VALUES (DEFAULT, 'LG UltraFine 27UP850N-W');

COMMIT;
```
