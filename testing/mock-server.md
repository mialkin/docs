# MockServer

[↑ MockServer](https://www.mock-server.com) is a tool that enables easy mocking of any system you integrate with via HTTP or HTTPS with clients written in Java, JavaScript and Ruby.

 MockServer also includes a proxy that introspects all proxied traffic including encrypted SSL traffic and supports port forwarding, web proxying (i.e. HTTP proxy), HTTPS tunneling proxying (using HTTP CONNECT).

[↑ JSON examples](https://github.com/mock-server/mockserver/blob/master/mockserver-examples/json_examples.md).

## `docker-compose`

```yml
version: "3.8"

services:
  mock-server:
    container_name: mock-server
    image: mockserver/mockserver:5.13.2
    ports:
      - 1080:1080
    environment:
      MOCKSERVER_PROPERTY_FILE: /mock-server/mock-server.properties
      MOCKSERVER_INITIALIZATION_JSON_PATH: /mock-server/*.json
    volumes:
      - type: bind
        source: ./mock-server
        target: /mock-server
    profiles: ["mockServer"]
```
