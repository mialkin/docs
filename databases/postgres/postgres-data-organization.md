# PostgreSQL data organization

[↑ PostgreSQL](https://www.postgresql.org/about/) is a program that belongs to the class of database management systems.

When this program is running, we call it a **PostgreSQL server**, or **instance**.

## Table of contents

- [PostgreSQL data organization](#postgresql-data-organization)
  - [Table of contents](#table-of-contents)
  - [Databases](#databases)
  - [System catalog](#system-catalog)
    - [`oid`](#oid)
  - [Schemas](#schemas)
  - [Tablespaces](#tablespaces)
  - [Relations](#relations)
  - [Files and forks](#files-and-forks)
  - [Pages](#pages)
  - [TOAST](#toast)

## Databases

Data managed by PostgreSQL is stored in databases. A single PostgreSQL instance can serve several databases at a time; together they are called a **database cluster**.

To be able to use the cluster, you must first _initialize_ (create) it. The directory that contains all the files related to the cluster is usually called `PGDATA`, after the name of the environment variable pointing to this directory.

```sql
SHOW data_directory;
--- or
SELECT setting FROM pg_settings WHERE name = 'data_directory';
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

## System catalog

Metadata of all cluster objects (such as tables, indexes, data types, or functions) is stored in tables that belong to the **system catalog**.

Each database has its own set of tables (and views) that describe the objects of this database. Several system catalog tables are common to the whole cluster; they do not belong to any particular database (technically, a dummy database with a zero ID is used), but can be accessed from all of them.

Names of all system catalog tables begin with `pg_`, like in `pg_database`. Column names start with a three-letter prefix that usually corresponds to the table name, like in `datname`.

[↑ system catalog](https://postgrespro.ru/docs/postgresql/15/catalogs?lang=en).

### `oid`

In all system catalog tables, the column declared as the primary key is called `oid` (object identifier); its type, which is also called `oid`, is a 32-bit integer.

> The implementation of `oid` object identifiers is virtually the same as that of sequences, but it appeared in PostgreSQL much earlier. What makes it special is that the generated unique IDs issued by a common counter are used in different tables of the system catalog. When an assigned ID exceeds the maximum value, the counter is reset. To ensure that all values in a particular table are unique, the next issued `oid` is checked by the unique index; if it is already used in this table, the counter is incremented, and the check is repeated.
>
> [↑ `GetNewOidWithIndex` function](https://git.postgresql.org/gitweb/?p=postgresql.git;a=blob;f=src/backend/catalog/catalog.c;hb=REL_16_STABLE).

## Schemas

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

## Tablespaces

Unlike databases and schemas, which determine logical distribution of objects, **tablespaces** define physical data layout. A tablespace is virtually a directory in a file system. You can distribute your data between tablespaces in such a way that archive data is stored on slow disks, while the data that is being actively updated goes to fast disks.

One and the same tablespace can be used by different databases, and each database can store data in several tablespaces. It means that logical structure and physical data layout do not depend on each other.

Each database has the so-called **default tablespace**. All database objects are created in this tablespace unless another location is specified. [System catalog](#system-catalog) objects related to this database are also stored there.

During cluster initialization, two tablespaces are created:

**pg_default** is located in the `PGDATA/base` directory; it is used as the default tablespace unless another tablespace is explicitly selected for this purpose.

**pg_global** is located in the `PGDATA/global` global directory; it stores system catalog objects that are common to the whole cluster.

When creating a custom tablespace, you can specify any directory; PostgreSQL will create a symbolic link to this location in the `PGDATA/pg_tblspc` directory. In fact, all paths used by PostgreSQL are relative to the `PGDATA` directory, which allows you to move it to a different location (provided that you have stopped the server, of course).

The illustration below puts together [databases](#databases), [schemas](#schemas), and tablespaces. Here the `postgres` database uses tablespace `xyzzy` as the default one, whereas the `template1` database uses `pg_default`. Various database objects are shown at the intersections of tablespaces and schemas.

<img src="images/0-tablespaces.png" width="600" alt="tablespaces" />

## Relations

For all of their differences, _tables_ and _indexes_ — the most important database objects — have one thing in common: they consist of rows. This point is quite self-evident when we think of tables, but it is equally true for B-tree nodes, which contain indexed values and references to other nodes or table rows.

Some other objects also have the same structure; for example, **sequences** (virtually one-row tables) and **materialized views** (which can be thought of as tables that "remember" the corresponding queries). Besides, there are regular **views**, which do not store any data but otherwise are very similar to tables.

In PostgreSQL, all these objects are referred to by the generic term **relation**.

The system catalog table for relations was originally called `pg_relation`, but following the object orientation trend, it was soon renamed to `pg_class`, which we are now used to. Its columns still have the `REL` prefix though.

## Files and forks

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

## Pages

To facilitate I/O, all files are logically split into **pages** (or **blocks**), which represent the minimum amount of data that can be read or written. Consequently, many internal PostgreSQL algorithms are tuned for page processing.

The page size is usually 8 KB. It can be configured to some extent (up to 32 KB), but only at build time (`./configure --with-blocksize`), and nobody usually does it. Once built and launched, the instance can work only with pages of the same size; it is impossible to create tablespaces that support different page sizes.

Regardless of the fork they belong to, all the files are handled by the server in roughly the same way. Pages are first moved to the buffer cache (where they can be read and updated by processes) and then flushed back to disk as required.

## TOAST

Each row must fit a single page: there is no way to continue a row on the next page. To store long rows, PostgreSQL uses a special mechanism called **TOAST** (The Oversized Attributes Storage Technique).

[↑ TOAST](https://postgrespro.ru/docs/postgresql/16/storage-toast?lang=en).

TOAST implies several strategies. You can move long attribute values into a separate service table, having sliced them into smaller "toasts." Another option is to compress a long value in such a way that the row fits the page. Or you can do both: first compress the value, and then slice and move it.

If the main table contains potentially long attributes, a separate TOAST table is created for it right away, one for all the attributes. For example, if a table has a column of the numeric or text type, a TOAST table will be created even if this column will never store any long values.

For indexes, the TOAST mechanism can offer only compression; moving long at- tributes into a separate table is not supported. It limits the size of the keys that can be indexed (the actual implementation depends on a particular operator class).

By default, the TOAST strategy is selected based on the data type of a column. The easiest way to review the used strategies is to run the `\d+` command in psql.

PostgreSQL supports the following strategies:

**plain** means that TOAST is not used (this strategy is applied to data types that are known to be “short,” such as the integer type).

**extended** allows both compressing attributes and storing them in a separate TOAST table.

**external** implies that long attributes are stored in the TOAST table in an uncompressed state.

**main** requires long attributes to be compressed first; they will be moved to the TOAST table only if compression did not help.

In general terms, the algorithm looks as follows. PostgreSQL aims at having at least four rows in a page. So if the size of the row exceeds one fourth of the page, excluding the header (for a standard-size page it is about 2000 bytes), we must apply the TOAST mechanism to some of the values. Following the workflow described below, we stop as soon as the row length does not exceed the threshold anymore:

1\. First of all, we go through attributes with external and extended strategies, starting from the longest ones. Extended attributes get compressed, and if the resulting value (on its own, without taking other attributes into account) exceeds one fourth of the page, it is moved to the TOAST table right away. External attributes are handled in the same way, except that the compression stage is skipped.

2\. If the row still does not fit the page after the first pass, we move the remaining attributes that use external or extended strategies into the TOAST table, one by one.

3\. If it did not help either, we try to compress the attributes that use the main strategy, keeping them in the table page.

4\. If the row is still not short enough, the main attributes are moved into the TOAST table.

The threshold value is 2000 bytes, but it can be redefined at the table level using the `toast_tuple_target` storage parameter.

It may sometimes be useful to change the default strategy for some of the columns. If it is known in advance that the data in a particular column cannot be compressed (for example, the column stores JPEG images), you can set the external strategy for this column; it allows you to avoid futile attempts to compress the data. The strategy can be changed as follows:

```sql
ALTER TABLE TABLE_NAME ALTER COLUMN COLUMN_NAME SET STORAGE external;
```

TOAST tables reside in a separate schema called `pg_toast`; it is not included into the search path, so TOAST tables are usually hidden. For temporary tables, `pg_toast_temp_N` schemas are used, by analogy with `pg_temp_N`.

Example of `SELECT` query against TOAST table:

```sql
SELECT chunk_id,
       chunk_seq,
       LENGTH(chunk_data),
       LEFT(ENCODE(chunk_data, 'escape')::text, 10) || '...' || RIGHT(ENCODE(chunk_data, 'escape')::text, 10)
FROM pg_toast.pg_toast_16521;
```

| chunk_id | chunk_seq | length | ?column?                |
| -------- | --------- | ------ | ----------------------- |
| 16526    | 0         | 1996   | RPVJKKFUXP...AWADGUMQYX |
| 16526    | 1         | 1996   | BOMXWYHTIR...QCUGCSBZHM |
| 16526    | 2         | 1008   | FAAIUDYLKC...HDBYJIKNOY |

We can see that the characters are sliced into chunks. The chunk size is selected in such a way that the page of the TOAST table can accommodate four rows. This value varies a little from version to version depending on the size of the page header.

When a long attribute is accessed, PostgreSQL automatically restores the original value and returns it to the client; it all happens seamlessly for the application. If long attributes do not participate in the query, the TOAST table will not be read at all. It is one of the reasons why you should avoid using the asterisk in production solutions.

If the client queries one of the first chunks of a long value, PostgreSQL will read the required chunks only, even if the value has been compressed.

Nevertheless, data compression and slicing require a lot of resources; the same goes for restoring the original values. That's why it is not a good idea to keep bulky data in PostgreSQL, especially if this data is being actively used and does not require transactional logic (like scanned accounting documents). A potentially better alternative is to store such data in the file system, keeping in the database only the names of the corresponding files. But then the database system cannot guarantee data consistency.
