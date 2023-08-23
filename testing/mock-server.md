# MockServer

[↑ MockServer](https://www.mock-server.com) is an API mocking tool.

API mocking involves creating a simple simulation of an API, accepting the same types of request and returning identically structured responses as the real thing, enabling fast and reliable development and testing.

API mocking is typically used during development and testing as it allows you to build your app without worrying about 3rd party APIs or sandboxes breaking. It can also be used to rapidly prototype APIs that don't exist yet.

## Local run

`docker-compose.yml` file:

```yaml
version: "3.9"

services:
  mockserver:
    image: mockserver/mockserver:5.15.0
    container_name: mockserver
    environment:
      MOCKSERVER_INITIALIZATION_JSON_PATH: /expectations.json
      MOCKSERVER_SERVER_PORT: ${MOCK_SERVER_PORT}
    ports:
      - "${MOCK_SERVER_PORT}:${MOCK_SERVER_PORT}"
    volumes:
      - ./docker/mockserver/expectations.json:/expectations.json:ro
```

`.env` file:

```env
MOCK_SERVER_PORT=2020
```

`expectations.json` file:

```json
[
  {
    "httpRequest": {
      "method": "POST",
      "path": "/api/users/get",
      "body": {
        "path": "/your_service/the/path",
        "service": "your_service"
      }
    },
    "httpResponse": {
      "statusCode": 200,
      "headers": [
        {
          "name": "Content-Type",
          "values": ["application/json"]
        }
      ],
      "body": {
        "type": "json",
        "json": {
          "response": {
            "items": [
              {
                "path": "/your_service/the/path",
                "type": "boolean",
                "value": true
              }
            ]
          }
        }
      }
    }
  }
]
```

cURL command:

```bash
curl --location 'http://localhost:2020/api/users/get' \
--header 'Content-Type: application/json' \
--data '{
    "path": "/your_service/the/path",
    "service": "your_service"
}'
```

cURL command response:

```json
{
  "response": {
    "items": [
      {
        "path": "/your_service/the/path",
        "type": "boolean",
        "value": true
      }
    ]
  }
}
```

## UI

MockServer UI: <http://localhost:2020/mockserver/dashboard>.

## Links

[↑ JSON examples](https://github.com/mock-server/mockserver/blob/master/mockserver-examples/json_examples.md).
