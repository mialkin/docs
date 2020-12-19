# Filebeat

## Installation using Docker

Create a `filebeat.yml` file:

```yml
filebeat.config:
  modules:
    path: ${path.config}/modules.d/*.yml
    reload.enabled: false

filebeat.inputs:
- type: log
  paths:
    - /slova/backuper/logs/*.log

filebeat.autodiscover:
  providers:
    - type: docker
      hints.enabled: true

processors:
- add_cloud_metadata: ~

output.elasticsearch:
  hosts: ['35.228.164.3:9200']
  username: 'elastic'
  password: 'oAjHldpjHWPgk3avJHY'

setup.kibana.host: "http://35.228.164.3:5601"
```

Change files' owner:

```bash
chown root filebeat.yml
```

`docker-compose.yml` file:

```yml
version: "3.8"
services:
  filebeat:
    image: docker.elastic.co/beats/filebeat:7.10.1
    user: root
    volumes:
      - ./filebeat.yml:/usr/share/filebeat/filebeat.yml:ro
      - /var/lib/docker/containers:/var/lib/docker/containers:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /slova/backuper/logs:/slova/backuper/logs:ro
    restart: unless-stopped
    environment:
      - strict.perms=false
    container_name: filebeat
```

Run container:

```bash
docker-compose up -d 
```

List available modules:

```bash
docker exec -it filebeat bash
./filebeat modules list
```

Load the recommended index template for writing to Elasticsearch and deploys the sample dashboards for visualizing the data in Kibana:

```bash
./filebeat setup -e
```

This step does not load the ingest pipelines used to parse log lines. By default, ingest pipelines are set up automatically the first time you run the module and connect to Elasticsearch.

Launch Kibana at http://35.228.164.3:5601. In the side navigation, click **Discover**. To see Filebeat data, make sure the predefined filebeat-* index pattern is selected.

## Links

[↑ Filebeat quick start: installation and configuration](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html)
