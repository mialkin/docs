# Microsoft SQL Server

**Microsoft SQL Server**, or **SQL Server** for short, is a proprietary relational database management system developed by Microsoft.

## Table of contents

- [Microsoft SQL Server](#microsoft-sql-server)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [Docker](#docker)
  - [Indexes](#indexes)
    - [Index scan](#index-scan)
    - [Index seek](#index-seek)
    - [Indexes with included columns](#indexes-with-included-columns)

## Running

### Docker

`docker-compose.yaml` file:

```yaml
version: "3.8"

services:
  sql-server:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: isolation-levels-sql-server
    ports:
      - "3400:1433"
    environment:
      ACCEPT_EULA: Y
      # Use sa/yourStrong(!)Password as user/password credentials
      MSSQL_SA_PASSWORD: yourStrong(!)Password
```

## Indexes

### Index scan

Since a scan touches every row in the table, whether or not it qualifies, the cost is proportional to the total number of rows in the table. Thus, a scan is an efficient strategy if the table is small or if most of the rows qualify for the predicate.

### Index seek

Index seek traverses a B-tree and walks through leaf nodes seeking only the matching or qualifying rows based on the filter criteria.

### Indexes with included columns

An index with included columns can greatly improve query performance because all columns in the query are included in the index. The query optimizer can locate all columns values within the index without accessing table or clustered index resulting in fewer disk I/O operations.

When an index contains all the columns referenced by a query, the index is typically referred to as *covering the query*.
