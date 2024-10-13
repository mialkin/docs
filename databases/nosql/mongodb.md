# MongoDB, Neo4j

## Table of contents

- [MongoDB, Neo4j](#mongodb-neo4j)
  - [Table of contents](#table-of-contents)
  - [MongoDB](#mongodb)
    - [Installation](#installation)
  - [Neo4j](#neo4j)
    - [Installation](#installation-1)
    - [Basic concepts](#basic-concepts)
    - [Cypher](#cypher)
      - [Commands](#commands)
      - [Social graph](#social-graph)

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

[↑ Neo4j for .NET](https://neo4j.com/docs/getting-started/languages-guides/neo4j-dotnet/).

### Installation

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

### Basic concepts

Neo4j stores data in a graph as _nodes_.

A **node** is a representation of an entity of a domain. The simplest graph has just a single node with some _properties_.

A **property** is a named value that adds qualities to nodes and relationships. Properties are simple key-value pairs.

A node can have zero or more _labels_.

A **label** is an abstraction that groups nodes into sets.

Neo4j is schema-free. Like any database, storing data in Neo4j can be as simple as adding more nodes.

A **relationship** is an abstraction that connects two nodes. Relationships always have a direction. Relationships always have a type.
Relationships form patterns of data, the structure of the graph.

In a property graph, relationships can also contain properties that describe the relationship.

A **relationship property** is an abstraction that stores information shared by two nodes.

### Cypher

[↑ Cypher](https://neo4j.com/docs/cypher-manual/current/introduction/cypher-overview/) is Neo4j's declarative graph query language.

It was created in 2011 by Neo4j engineers as an SQL-equivalent language for graph databases. Similar to SQL, Cypher lets users focus on _what_ to retrieve from graph, rather than _how_ to retrieve it. Cypher uses patterns to describe graph data.

#### Commands

| Command  | Description                                               |
| -------- | --------------------------------------------------------- |
| :clear   | Clear the stream of result frames                         |
| :history | Bring up the history of the executed commands and queries |

#### Social graph

Let's use Cypher to generate a small social graph:

```sql
CREATE (ee:Person {name: 'Emil', from: 'Sweden', kloutScore: 99})
```

- `CREATE` creates the node
- `()` indicates the node
- `ee:Person` – `ee` is the node variable and `Person` is the node label
- `{}` contains the properties that describe the node

Find the node representing Emil:

```sql
MATCH (ee:Person) WHERE ee.name = 'Emil' RETURN ee;
```

- `MATCH` specifies a pattern of nodes and relationships (`ee:Person`) is a single node pattern with label `Person`. It assigns matches to the variable `ee`
- `WHERE` filters the query
- `ee.name = 'Emil'` compares name property to the value `Emil`
- `RETURN` returns particular results

The `CREATE` clause can create many nodes and relationships at once:

```sql
CREATE (js:Person { name: 'Johan', from: 'Sweden', learn: 'surfing' }),
(ir:Person { name: 'Ian', from: 'England', title: 'author' }),
(rvb:Person { name: 'Rik', from: 'Belgium', pet: 'Orval' }),
(ally:Person { name: 'Allison', from: 'California', hobby: 'surfing' }),
(ee)-[:KNOWS {since: 2001}]->(js),(ee)-[:KNOWS {rating: 5}]->(ir),
(js)-[:KNOWS]->(ir),(js)-[:KNOWS]->(rvb),
(ir)-[:KNOWS]->(js),(ir)-[:KNOWS]->(ally),
(rvb)-[:KNOWS]->(ally)
```

A pattern that can be used to find Emil's friends:

```sql
MATCH (ee:Person)-[:KNOWS]-(friends)
WHERE ee.name = 'Emil' RETURN ee, friends
```

- `MATCH` describes what nodes will be retrieved based upon the pattern
- `(ee)` is the node reference that will be returned based upon the `WHERE` clause
- `-[:KNOWS]-` matches the `KNOWS` relationships (in either direction) from `ee`
- `(friends)` represents the nodes that are Emil's friends
- `RETURN` returns the node, referenced here by `(ee)`, and the related `(friends)` nodes found
