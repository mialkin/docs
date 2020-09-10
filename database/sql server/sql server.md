# SQL Server

Run container.

```sh
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=yourStrong(!)Password' -p 1433:1433 -d mcr.microsoft.com/mssql/server
```

Download AdventureWorks sample database and copy it to the container.

```sh
docker cp /Users/aleksei/Downloads/AdventureWorksLT2019.bak fd14:tmp/AdventureWorksLT2019.bak
```
