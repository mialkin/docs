# Heartbeat

```yml
heartbeat.monitors:
- type: http
  schedule: '@every 5s'
  urls:
    - http://35.228.164.3:9200
    - http://35.228.164.3:5601

- type: icmp
  schedule: '@every 5s'
  hosts:
    - elasticsearch
    - kibana

processors:
- add_cloud_metadata: ~

output.elasticsearch:
  hosts: ['35.228.164.3:9200']
  username: 'elastic'
  password: 'oAjHldpjHWPgk3avJHY'
```

```bash
docker pull docker.elastic.co/beats/heartbeat:7.10.1
```

```bash
docker run -d \
  --name=heartbeat \
  --user=heartbeat \
  --volume="$(pwd)/heartbeat.docker.yml:/usr/share/heartbeat/heartbeat.yml:ro" \
  docker.elastic.co/beats/heartbeat:7.10.1 \
  --strict.perms=false -e
```
