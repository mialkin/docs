# Grafana

**Grafana** is an open source visualization and analytics platform.

## Run

```bash
docker run -d -p 3000:3000 --name=grafana grafana/grafana-enterprise
```

## Connect to Prometheus

**Configuration** (gear icon) → **Data Sources** → `http://host.docker.internal:9090`