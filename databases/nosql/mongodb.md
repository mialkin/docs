# MongoDB

[↑ MongoDB](https://www.mongodb.com/docs/) is a source-available, cross-platform, document-oriented database program.

Classified as a NoSQL database product, MongoDB utilizes JSON-like documents with optional schemas.

## Installation

Running MongoDB via `docker-compose.yaml` file:

```yaml
services:
  mongodb:
    image: mongodb/mongodb-community-server:7.0-ubi9
    ports:
      - 27017:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: example
```

## Compass

[↑ Compass](https://www.mongodb.com/products/tools/compass) is a free interactive tool for querying, optimizing, and analyzing MongoDB data.
