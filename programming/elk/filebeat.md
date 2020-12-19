# Filebeat

## Installation using Docker

Create a `filebeat.docker.yml` file:

```yml
filebeat.config:
  modules:
    path: ${path.config}/modules.d/*.yml
    reload.enabled: false

filebeat.inputs:
- type: log
  paths:
    - /slova/logs/backuper/*.log


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

Run container:

```bash
docker run -d \
  --name=filebeat \
  --user=root \
  --volume="$(pwd)/filebeat.docker.yml:/usr/share/filebeat/filebeat.yml:ro" \
  --volume="/var/lib/docker/containers:/var/lib/docker/containers:ro" \
  --volume="/var/run/docker.sock:/var/run/docker.sock:ro" \
  --volume="/Users/aleksei/repositories/slova.io/Slova.Backuper/src/Slova.Backuper/bin/Debug/net5.0/logs:/slova/logs/backuper:ro" \
  docker.elastic.co/beats/filebeat:7.10.1 filebeat -e -strict.perms=false
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
