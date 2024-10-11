# MongoDB, Neo4j

## Table of contents

- [MongoDB, Neo4j](#mongodb-neo4j)
  - [Table of contents](#table-of-contents)
  - [MongoDB](#mongodb)
    - [Installation](#installation)
  - [Neo4j](#neo4j)
    - [Cypher](#cypher)

## MongoDB

[↑ MongoDB](https://www.mongodb.com/docs/) is a source-available, cross-platform, document-oriented database program.

Classified as a NoSQL database product, MongoDB utilizes JSON-like documents with optional schemas.

### Installation

Running MongoDB via `compose.yaml` file:

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

[↑ Compass](https://www.mongodb.com/products/tools/compass) is a free interactive tool for querying, optimizing, and analyzing MongoDB data.

## Neo4j

[↑ Neo4j](https://github.com/neo4j/neo4j) is a graph database management system, GDBMS.

```yaml
services:
  neo4j:
    image: neo4j:5.24.1-ubi9
    container_name: neo4j
    restart: unless-stopped
    ports:
      - 7474:7474
      - 7687:7687
    environment:
      NEO4J_AUTH: neo4j/your_password123
    volumes:
      - ./volumes/neo4j/data:/data
```

### Cypher

[↑ Cypher](https://neo4j.com/docs/cypher-manual/current/introduction/cypher-overview/) is Neo4j's declarative graph query language.

It was created in 2011 by Neo4j engineers as an SQL-equivalent language for graph databases. Similar to SQL, Cypher lets users focus on _what_ to retrieve from graph, rather than _how_ to retrieve it.
