# Metricbeat

Pull Docker image:

```bash
docker pull docker.elastic.co/beats/metricbeat:7.10.1
```

Pull configuration file:

```bash
curl -L -O https://raw.githubusercontent.com/elastic/beats/7.10/deploy/docker/metricbeat.docker.yml
```

Setup configuration file:

```yml
metricbeat.config:
  modules:
    path: ${path.config}/modules.d/*.yml
    # Reload module configs as they change:
    reload.enabled: false

metricbeat.autodiscover:
  providers:
    - type: docker
      hints.enabled: true

metricbeat.modules:
- module: docker
  metricsets:
    - "container"
    - "cpu"
    - "diskio"
    - "healthcheck"
    - "info"
    #- "image"
    - "memory"
    - "network"
  hosts: ["unix:///var/run/docker.sock"]
  period: 10s
  enabled: true

processors:
  - add_cloud_metadata: ~

output.elasticsearch:
  hosts: ['35.228.164.3:9200']
  username: 'elastic'
  password: 'oAjHldpjHWPgk3avJHY'
```

Run container:

```bash
docker run -d \
  --name=metricbeat \
  --user=root \
  --volume="$(pwd)/metricbeat.docker.yml:/usr/share/metricbeat/metricbeat.yml:ro" \
  --volume="/var/run/docker.sock:/var/run/docker.sock:ro" \
  --volume="/sys/fs/cgroup:/hostfs/sys/fs/cgroup:ro" \
  --volume="/proc:/hostfs/proc:ro" \
  --volume="/:/hostfs:ro" \
  docker.elastic.co/beats/metricbeat:7.10.1 metricbeat -e -strict.perms=false
```
