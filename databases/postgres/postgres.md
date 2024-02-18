# PostgreSQL

[↑ PostgreSQL](https://www.postgresql.org/about/) is a program that belongs to the class of database management systems.

When this program is running, we call it a **PostgreSQL server**, or **instance**.

## Table of contents

- [PostgreSQL](#postgresql)
  - [Table of contents](#table-of-contents)
  - [Data organization](#data-organization)
    - [Databases](#databases)
    - [System catalog](#system-catalog)
      - [`oid`](#oid)

## Data organization

### Databases

Data managed by PostgreSQL is stored in databases. A single PostgreSQL instance can serve several databases at a time; together they are called a **database cluster**.

To be able to use the cluster, you must first _initialize_ (create) it. The directory that contains all the files related to the cluster is usually called `PGDATA`, after the name of the environment variable pointing to this directory.

After cluster initialization, `PGDATA` contains three identical databases:

**template0** is used for cases like restoring data from a logical backup or creating a
database with a different encoding; it must never be modified.

**template1** serves as a template for all the other databases that a user can create in the cluster.

**postgres** is a regular database that you can use at your discretion.

### System catalog

Metadata of all cluster objects (such as tables, indexes, data types, or functions) is stored in tables that belong to the **system catalog**.

Each database has its own set of tables (and views) that describe the objects of this database. Several system catalog tables are common to the whole cluster; they do not belong to any particular database (technically, a dummy database with a zero ID is used), but can be accessed from all of them.

Names of all system catalog tables begin with `pg_`, like in `pg_database`. Column names start with a three-letter prefix that usually corresponds to the table name, like in `datname`.

[↑ system catalog](https://postgrespro.ru/docs/postgresql/15/catalogs?lang=en).

#### `oid`

In all system catalog tables, the column declared as the primary key is called `oid` (object identifier); its type, which is also called `oid`, is a 32-bit integer.

> The implementation of oid object identifiers is virtually the same as that of sequences, but it appeared in PostgreSQL much earlier. What makes it special is that the generated unique IDs issued by a common counter are used in different tables of the system catalog. When an assigned ID exceeds the maximum value, the counter is reset. To ensure that all values in a particular table are unique, the next issued oid is checked by the unique index; if it is already used in this table, the counter is incremented, and the check is repeated.
> 
> [↑ `GetNewOidWithIndex` function](https://git.postgresql.org/gitweb/?p=postgresql.git;a=blob;f=src/backend/catalog/catalog.c;hb=REL_16_STABLE).
