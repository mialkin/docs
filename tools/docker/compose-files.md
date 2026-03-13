# Compose files

## Table of contents

- [Compose files](#compose-files)
  - [Table of contents](#table-of-contents)
  - [Postgres](#postgres)

## Postgres

```yaml
services:
  database:
    image: postgres:18.3
    container_name: postgres
    restart: unless-stopped
    ports:
      - 8420:5432
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    volumes:
      - ./volumes/postgres:/var/lib/postgresql/18/docker
```

```bash
docker exec -it postgres psql -U postgres
```
