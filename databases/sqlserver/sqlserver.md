# Microsoft SQL Server

## TOC

- [Microsoft SQL Server](#microsoft-sql-server)
  - [TOC](#toc)
  - [Installation](#installation)
    - [Docker](#docker)

## Installation

### Docker

```bash
docker run --name sqlserver -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=yourStr@ngPassw0rd" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
