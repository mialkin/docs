# Microsoft SQL Server

## TOC

- [Microsoft SQL Server](#microsoft-sql-server)
  - [TOC](#toc)
  - [Installation](#installation)
    - [Docker](#docker)
  - [Indexes](#indexes)
    - [Index scan](#index-scan)
    - [Index seek](#index-seek)
    - [Indexes with included columns](#indexes-with-included-columns)

## Installation

### Docker

```bash
docker run --name sqlserver -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=yourStr@ngPassw0rd" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

## Indexes

### Index scan

Since a scan touches every row in the table, whether or not it qualifies, the cost is proportional to the total number of rows in the table. Thus, a scan is an efficient strategy if the table is small or if most of the rows qualify for the predicate.

### Index seek

Index seek traverses a B-tree and walks through leaf nodes seeking only the matching or qualifying rows based on the filter criteria.

### Indexes with included columns

An index with included columns can greatly improve query performance because all columns in the query are included in the index; The query optimizer can locate all columns values within the index without accessing table or clustered index resulting in fewer disk I/O operations.

> When an index contains all the columns referenced by a query, the index is typically referred to as *covering the query*.
