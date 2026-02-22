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

## Table of contents

- [EAV, entity–attribute–value model](#eav-entityattributevalue-model)
  - [Table of contents](#table-of-contents)
  - [Example](#example)
    - [Creating tables](#creating-tables)
    - [Adding data](#adding-data)
    - [Querying](#querying)
  - [Alternatives](#alternatives)

## Example

### Creating tables

```sql
-- 1. The Entity Table: General info common to all products
CREATE TABLE products
(
    id         SERIAL PRIMARY KEY,
    sku        TEXT UNIQUE NOT NULL,
    name       TEXT        NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 2. The Attribute Table: Definitions of possible specs
CREATE TABLE product_attributes
(
    id        SERIAL PRIMARY KEY,
    name      TEXT NOT NULL UNIQUE, -- e.g., 'Color', 'Resolution', 'RAM'
    data_type TEXT NOT NULL         -- useful for application-side validation
);

-- 3. The Value Table (EAV Table): The "long" table
CREATE TABLE product_attribute_values
(
    id           SERIAL PRIMARY KEY,
    product_id   INTEGER REFERENCES products (id) ON DELETE CASCADE,
    attribute_id INTEGER REFERENCES product_attributes (id) ON DELETE CASCADE,
    value        TEXT NOT NULL, -- Stored as text, casted by the app

    -- Ensure a product doesn't have the same attribute twice
    UNIQUE (product_id, attribute_id)
);

```

### Adding data

```sql
-- Insert Attributes
INSERT INTO product_attributes (name, data_type)
VALUES ('Color', 'string'),
       ('Screen Size', 'string'),
       ('Storage', 'integer');
```

`product_attributes`:

| id  | name        | data_type |
| :-- | :---------- | :-------- |
| 1   | Color       | string    |
| 2   | Screen Size | string    |
| 3   | Storage     | integer   |

```sql
-- Insert a Laptop
INSERT INTO products (sku, name)
VALUES ('LAP-001', 'MacBook Pro');
INSERT INTO product_attribute_values (product_id, attribute_id, value)
VALUES (1, 1, 'Space Gray'),
       (1, 2, '14-inch'),
       (1, 3, '512GB');

-- Insert a T-Shirt (Completely different attributes)
INSERT INTO products (sku, name)
VALUES ('TSH-002', 'Cotton Tee');
INSERT INTO product_attribute_values (product_id, attribute_id, value)
VALUES (2, 1, 'Blue');
```

`products`:

| id  | sku     | name        | created_at                 |
| :-- | :------ | :---------- | :------------------------- |
| 1   | LAP-001 | MacBook Pro | 2026-02-21 21:10:16.429827 |
| 2   | TSH-002 | Cotton Tee  | 2026-02-21 21:10:16.460986 |

`product_attribute_values`:

| id  | product_id | attribute_id | value      |
| :-- | :--------- | :----------- | :--------- |
| 1   | 1          | 1            | Space Gray |
| 2   | 1          | 2            | 14-inch    |
| 3   | 1          | 3            | 512GB      |
| 4   | 2          | 1            | Blue       |

### Querying

```sql
select p.name, pa.name, pa.data_type, pav.value
from product_attribute_values pav
         join product_attributes pa
              on pav.attribute_id = pa.id
         join products p on pav.product_id = p.id
where p.id = 1;
```

| name        | name        | data_type | value      |
| :---------- | :---------- | :-------- | :--------- |
| MacBook Pro | Color       | string    | Space Gray |
| MacBook Pro | Screen Size | string    | 14-inch    |
| MacBook Pro | Storage     | integer   | 512GB      |

## Alternatives

An alternative could be JSONB in Postgres, or MongoDB, but later does not support transactions.

The EAV model is a [↑ slippery slope](https://stackoverflow.com/questions/494158/best-beginner-resources-for-understanding-the-eav-database-model). It can work fine in a small tightly-scoped database, but blows up in your face the nearer you get to a production system.

[↑ Entity-attribute-value (EAV) design in PostgreSQL - don't do it!](https://www.cybertec-postgresql.com/en/entity-attribute-value-eav-design-in-postgresql-dont-do-it/).

[↑ Bad CaRMa](https://www.red-gate.com/simple-talk/opinion/opinion-pieces/bad-carma).

[↑ EAV Zero to EAV Hero!](https://www.youtube.com/watch?v=WneHTRZVbec).
