# Microsoft SQL Server

**Microsoft SQL Server**, or **SQL Server** for short, is a proprietary relational database management system developed by Microsoft.

## Table of contents

- [Microsoft SQL Server](#microsoft-sql-server)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [Docker](#docker)

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
