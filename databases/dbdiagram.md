# dbdiagram.io

[↑ dbdiagram.io](https://dbdiagram.io) is a free, simple tool to draw ER diagrams by just writing code.

Export tool at the top of the UI can be used to save diagrams as a PDF or PNG files, or generate SQL codes for Postgres, MySQL or SQL server.

## Table of contents

- [dbdiagram.io](#dbdiagramio)
  - [Table of contents](#table-of-contents)
  - [DBML](#dbml)
    - [MySQL generated code](#mysql-generated-code)
    - [Postgres generated code](#postgres-generated-code)
    - [SQL Server generated code](#sql-server-generated-code)

## DBML

[↑ Database Markup Language](https://dbml.dbdiagram.io/docs) or **DBML** is an open-source DSL language designed to define and document database schemas and structures.

dbdiagram.io uses DBML as its syntax for defining database.

DBML example:

```text
Table accounts as A {
  id bigserial [pk]
  owner varchar [not null]
  balance bigint [not null]
  currency varchar [not null]
  created_at timestamptz [not null, default: `now()`]

  Indexes {
    owner
  }
}

Table entries {
  id bigserial [pk]
  account_id bigint [ref: > A.id, not null]
  amount bigint [not null, note: 'can be negative or positive']
  created_at timestamptz [not null, default: `now()`]

  Indexes {
    account_id
  }
}

Table transfers {
  id bigserial [pk]
  from_account_id bigint [ref: > A.id, not null]
  to_account_id bigint [ref: > A.id, not null]
  amount bigint [not null, note: 'must be positive']
  created_at timestamptz [not null, default: `now()`]

  Indexes {
    from_account_id
    to_account_id
    (from_account_id, to_account_id)
  }
}
```

### MySQL generated code

```sql
CREATE TABLE `accounts` (
  `id` bigserial PRIMARY KEY,
  `owner` varchar(255) NOT NULL,
  `balance` bigint NOT NULL,
  `currency` varchar(255) NOT NULL,
  `created_at` timestamptz NOT NULL DEFAULT (now())
);

CREATE TABLE `entries` (
  `id` bigserial PRIMARY KEY,
  `account_id` bigint NOT NULL,
  `amount` bigint NOT NULL COMMENT 'can be negative or positive',
  `created_at` timestamptz NOT NULL DEFAULT (now())
);

CREATE TABLE `transfers` (
  `id` bigserial PRIMARY KEY,
  `from_account_id` bigint NOT NULL,
  `to_account_id` bigint NOT NULL,
  `amount` bigint NOT NULL COMMENT 'must be positive',
  `created_at` timestamptz NOT NULL DEFAULT (now())
);

CREATE INDEX `accounts_index_0` ON `accounts` (`owner`);

CREATE INDEX `entries_index_1` ON `entries` (`account_id`);

CREATE INDEX `transfers_index_2` ON `transfers` (`from_account_id`);

CREATE INDEX `transfers_index_3` ON `transfers` (`to_account_id`);

CREATE INDEX `transfers_index_4` ON `transfers` (`from_account_id`, `to_account_id`);

ALTER TABLE `entries` ADD FOREIGN KEY (`account_id`) REFERENCES `accounts` (`id`);

ALTER TABLE `transfers` ADD FOREIGN KEY (`from_account_id`) REFERENCES `accounts` (`id`);

ALTER TABLE `transfers` ADD FOREIGN KEY (`to_account_id`) REFERENCES `accounts` (`id`);
```

### Postgres generated code

```sql
CREATE TABLE "accounts" (
  "id" bigserial PRIMARY KEY,
  "owner" varchar NOT NULL,
  "balance" bigint NOT NULL,
  "currency" varchar NOT NULL,
  "created_at" timestamptz NOT NULL DEFAULT (now())
);

CREATE TABLE "entries" (
  "id" bigserial PRIMARY KEY,
  "account_id" bigint NOT NULL,
  "amount" bigint NOT NULL,
  "created_at" timestamptz NOT NULL DEFAULT (now())
);

CREATE TABLE "transfers" (
  "id" bigserial PRIMARY KEY,
  "from_account_id" bigint NOT NULL,
  "to_account_id" bigint NOT NULL,
  "amount" bigint NOT NULL,
  "created_at" timestamptz NOT NULL DEFAULT (now())
);

CREATE INDEX ON "accounts" ("owner");

CREATE INDEX ON "entries" ("account_id");

CREATE INDEX ON "transfers" ("from_account_id");

CREATE INDEX ON "transfers" ("to_account_id");

CREATE INDEX ON "transfers" ("from_account_id", "to_account_id");

COMMENT ON COLUMN "entries"."amount" IS 'can be negative or positive';

COMMENT ON COLUMN "transfers"."amount" IS 'must be positive';

ALTER TABLE "entries" ADD FOREIGN KEY ("account_id") REFERENCES "accounts" ("id");

ALTER TABLE "transfers" ADD FOREIGN KEY ("from_account_id") REFERENCES "accounts" ("id");

ALTER TABLE "transfers" ADD FOREIGN KEY ("to_account_id") REFERENCES "accounts" ("id");
```

### SQL Server generated code

```sql
CREATE TABLE [accounts] (
  [id] bigserial PRIMARY KEY,
  [owner] nvarchar(255) NOT NULL,
  [balance] bigint NOT NULL,
  [currency] nvarchar(255) NOT NULL,
  [created_at] timestamptz NOT NULL DEFAULT (now())
)
GO

CREATE TABLE [entries] (
  [id] bigserial PRIMARY KEY,
  [account_id] bigint NOT NULL,
  [amount] bigint NOT NULL,
  [created_at] timestamptz NOT NULL DEFAULT (now())
)
GO

CREATE TABLE [transfers] (
  [id] bigserial PRIMARY KEY,
  [from_account_id] bigint NOT NULL,
  [to_account_id] bigint NOT NULL,
  [amount] bigint NOT NULL,
  [created_at] timestamptz NOT NULL DEFAULT (now())
)
GO

CREATE INDEX [accounts_index_0] ON [accounts] ("owner")
GO

CREATE INDEX [entries_index_1] ON [entries] ("account_id")
GO

CREATE INDEX [transfers_index_2] ON [transfers] ("from_account_id")
GO

CREATE INDEX [transfers_index_3] ON [transfers] ("to_account_id")
GO

CREATE INDEX [transfers_index_4] ON [transfers] ("from_account_id", "to_account_id")
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'can be negative or positive',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'entries',
@level2type = N'Column', @level2name = 'amount';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'must be positive',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'transfers',
@level2type = N'Column', @level2name = 'amount';
GO

ALTER TABLE [entries] ADD FOREIGN KEY ([account_id]) REFERENCES [accounts] ([id])
GO

ALTER TABLE [transfers] ADD FOREIGN KEY ([from_account_id]) REFERENCES [accounts] ([id])
GO

ALTER TABLE [transfers] ADD FOREIGN KEY ([to_account_id]) REFERENCES [accounts] ([id])
GO
```
