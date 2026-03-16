# Compose files

## Table of contents

- [Compose files](#compose-files)
  - [Table of contents](#table-of-contents)
  - [`grafana/otel-lgtm`](#grafanaotel-lgtm)
  - [Postgres](#postgres)
  - [Redis](#redis)

## `grafana/otel-lgtm`

```yaml
services:
  otel-lgtm:
    image: grafana/otel-lgtm:latest
    container_name: otel-lgtm
    restart: unless-stopped
    ports:
      - 3000:3000 # Grafana UI. User: admin, password: admin
      - 4317:4317 # OpenTelemetry gRPC endpoint
      - 4318:4318 # OpenTelemetry HTTP endpoint
      - 4040:4040 # Pyroscope endpoint
      - 9090:9090 # Prometheus endpoint
```

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

## Redis

```yaml
services:
  redis:
    image: redis:8.6.1
    container_name: redis-server
    restart: unless-stopped
    ports:
      - 6379:6379
    volumes:
      - redis_data:/data
    command: redis-server --appendonly yes

volumes:
  redis_data:
```

```bash
docker exec -it redis-server redis-cli
```
