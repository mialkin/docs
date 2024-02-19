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
    - [Schemas](#schemas)
    - [Tablespaces](#tablespaces)
    - [Relations](#relations)
    - [Files and forks](#files-and-forks)
    - [Pages](#pages)
    - [TOAST](#toast)

## Data organization

### Databases

Data managed by PostgreSQL is stored in databases. A single PostgreSQL instance can serve several databases at a time; together they are called a **database cluster**.

To be able to use the cluster, you must first *initialize* (create) it. The directory that contains all the files related to the cluster is usually called `PGDATA`, after the name of the environment variable pointing to this directory.

```sql
SHOW data_directory;
--- or
SELECT setting
FROM pg_settings
WHERE name = 'data_directory';
```

For Postgres 16, installed on macOS with `brew install postgresql@16`, `PGDATA` directory is:

```text
/usr/local/var/postgresql@16
```

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

> The implementation of `oid` object identifiers is virtually the same as that of sequences, but it appeared in PostgreSQL much earlier. What makes it special is that the generated unique IDs issued by a common counter are used in different tables of the system catalog. When an assigned ID exceeds the maximum value, the counter is reset. To ensure that all values in a particular table are unique, the next issued `oid` is checked by the unique index; if it is already used in this table, the counter is incremented, and the check is repeated.
>
> [↑ `GetNewOidWithIndex` function](https://git.postgresql.org/gitweb/?p=postgresql.git;a=blob;f=src/backend/catalog/catalog.c;hb=REL_16_STABLE).

### Schemas

**Schemas** are namespaces that store all objects of a database.

Apart from user schemas, PostgreSQL offers several predefined ones:

**public** is the default schema for user objects unless other settings are specified.

**pg_catalog** is used for [system catalog](#system-catalog) tables.

**information_schema** provides an alternative view for the system catalog as defined by the SQL standard.

**pg_toast** is used for objects related to [TOAST](#toast).

**pg_temp** comprises temporary tables. Although different users create temporary tables in different schemas called `pg_temp_N`, everyone refers to their objects using the `pg_temp` alias.

Each schema is confined to a particular database, and all database objects belong to this or that schema.

If the schema is not specified explicitly when an object is accessed, PostgreSQL selects the first suitable schema from the **search path**. The search path is based on the value of the `search_path` parameter, which is implicitly extended with `pg_catalog` and (if necessary) `pg_temp` schemas. It means that different schemas can contain objects with the same names.

```sql
SHOW search_path;
```

[↑ schemas](https://postgrespro.ru/docs/postgresql/15/ddl-schemas?lang=en).

### Tablespaces

Unlike databases and schemas, which determine logical distribution of objects, **tablespaces** define physical data layout. A tablespace is virtually a directory in a file system. You can distribute your data between tablespaces in such a way that archive data is stored on slow disks, while the data that is being actively updated goes to fast disks.

One and the same tablespace can be used by different databases, and each database can store data in several tablespaces. It means that logical structure and physical data layout do not depend on each other.

Each database has the so-called **default tablespace**. All database objects are created in this tablespace unless another location is specified. [System catalog](#system-catalog) objects related to this database are also stored there.

During cluster initialization, two tablespaces are created:

**pg_default** is located in the `PGDATA/base` directory; it is used as the default tablespace unless another tablespace is explicitly selected for this purpose.

**pg_global** is located in the `PGDATA/global` global directory; it stores system catalog objects that are common to the whole cluster.

When creating a custom tablespace, you can specify any directory; PostgreSQL will create a symbolic link to this location in the `PGDATA/pg_tblspc` directory. In fact, all paths used by PostgreSQL are relative to the `PGDATA` directory, which allows you to move it to a different location (provided that you have stopped the server, of course).

The illustration below puts together [databases](#databases), [schemas](#schemas), and tablespaces. Here the `postgres` database uses tablespace `xyzzy` as the default one, whereas the `template1` database uses `pg_default`. Various database objects are shown at the intersections of tablespaces and schemas.

<img src="images/0-tablespaces.png" width="600" alt="tablespaces" />

### Relations

For all of their differences, *tables* and *indexes* — the most important database objects — have one thing in common: they consist of rows. This point is quite self-evident when we think of tables, but it is equally true for B-tree nodes, which contain indexed values and references to other nodes or table rows.

Some other objects also have the same structure; for example, **sequences** (virtually one-row tables) and **materialized views** (which can be thought of as tables that "remember" the corresponding queries). Besides, there are regular **views**, which do not store any data but otherwise are very similar to tables.

In PostgreSQL, all these objects are referred to by the generic term **relation**.

The system catalog table for relations was originally called `pg_relation`, but following the object orientation trend, it was soon renamed to `pg_class`, which we are now used to. Its columns still have the `REL` prefix though.

### Files and forks

All information associated with a relation is stored in several different **forks**, each containing data of a particular type.

At first, a fork is represented by a single file. Its filename consists of a numeric ID (`oid`), which can be extended by a suffix that corresponds to the fork's type.

The file grows over time, and when its size reaches 1 GB, another file of this fork is created (such files are sometimes called **segments**). The sequence number of the segment is added to the end of its filename.

The file size limit of 1 GB was historically established to support various file systems that could not handle large files. You can change this limit when building PostgreSQL (`./configure --with-segsize`).

Thus, a single relation is represented on disk by several files. Even a small table without indexes will have at least three files, by the number of mandatory forks.

Each [tablespace](#tablespaces) directory (except for `pg_global`) contains separate subdirectories for particular databases. All files of the objects belonging to the same tablespace and database are located in the same subdirectory. You must take it into account because too many files in a single directory may not be handled well by file systems.

[↑ Database File Layout](https://postgrespro.ru/docs/postgresql/16/storage-file-layout?lang=en).

<img src="images/0-forks.png" width="600" alt="tablespaces" />

There are several standard types of forks.

The **main fork** represents actual data: table rows or index rows. This fork is available for any relations (except for views, which contain no data).

The **initialization fork** is available only for unlogged tables (created with the `UNLOGGED` clause) and their indexes. Such objects are the same as regular ones, except that any actions performed on them are not written into the write-ahead log. It makes these operations considerably faster, but you will not be able to restore consistent data in case of a failure. Therefore, PostgreSQL simply deletes all forks of such objects during recovery and overwrites the main fork with the initialization fork, thus creating a dummy file.

[↑The Initialization Fork](https://postgrespro.ru/docs/postgresql/16/storage-init?lang=en).

The **free space map** keeps track of available space within pages. Its volume changes all the time, growing after vacuuming and getting smaller when new row versions appear. The free space map is used to quickly find a page that can accommodate new data being inserted.

[↑ Free Space Map](https://postgrespro.ru/docs/postgresql/16/storage-fsm?lang=en).

The **visibility map** can quickly show whether a page needs to be vacuumed or frozen. For this purpose, it provides two bits for each table page.

[↑ Visibility Map](https://postgrespro.ru/docs/postgresql/16/storage-vm?lang=en).

### Pages

To facilitate I/O, all files are logically split into **pages** (or **blocks**), which represent the minimum amount of data that can be read or written. Consequently, many internal PostgreSQL algorithms are tuned for page processing.

The page size is usually 8 KB. It can be configured to some extent (up to 32 KB), but only at build time (`./configure --with-blocksize`), and nobody usually does it. Once built and launched, the instance can work only with pages of the same size; it is impossible to create tablespaces that support different page sizes.

Regardless of the fork they belong to, all the files are handled by the server in roughly the same way. Pages are first moved to the buffer cache (where they can be read and updated by processes) and then flushed back to disk as required.

### TOAST
