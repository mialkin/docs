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
      - [The Movie Graph](#the-movie-graph)

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

#### The Movie Graph

The Movie Graph is a mini graph application, containing actors and directors that are related through the movies they have collaborated on.

This guide shows how to:

- Load: Insert movie data into the graph
- Constrain: Create unique node property constraints
- Index: Index nodes based on their labels
- Find: Retrieve individual movies and actors
- Query: Discover related actors and directors
- Solve: The Bacon Path

Delete all `Movie` and `Person` nodes, and their relationships:

```sql
MATCH (n) DETACH DELETE n
```

Verify that the Movie Graph has been removed:

```sql
MATCH (n) RETURN n
```

Use the following code block to create the movie graph. It contains a single Cypher query statement composed of multiple `CREATE` clauses.

```sql
CREATE (TheMatrix:Movie {title:'The Matrix', released:1999, tagline:'Welcome to the Real World'})
CREATE (Keanu:Person {name:'Keanu Reeves', born:1964})
CREATE (Carrie:Person {name:'Carrie-Anne Moss', born:1967})
CREATE (Laurence:Person {name:'Laurence Fishburne', born:1961})
CREATE (Hugo:Person {name:'Hugo Weaving', born:1960})
CREATE (LillyW:Person {name:'Lilly Wachowski', born:1967})
CREATE (LanaW:Person {name:'Lana Wachowski', born:1965})
CREATE (JoelS:Person {name:'Joel Silver', born:1952})
CREATE
(Keanu)-[:ACTED_IN {roles:['Neo']}]->(TheMatrix),
(Carrie)-[:ACTED_IN {roles:['Trinity']}]->(TheMatrix),
(Laurence)-[:ACTED_IN {roles:['Morpheus']}]->(TheMatrix),
(Hugo)-[:ACTED_IN {roles:['Agent Smith']}]->(TheMatrix),
(LillyW)-[:DIRECTED]->(TheMatrix),
(LanaW)-[:DIRECTED]->(TheMatrix),
(JoelS)-[:PRODUCED]->(TheMatrix)

CREATE (Emil:Person {name:"Emil Eifrem", born:1978})
CREATE (Emil)-[:ACTED_IN {roles:["Emil"]}]->(TheMatrix)

CREATE (TheMatrixReloaded:Movie {title:'The Matrix Reloaded', released:2003, tagline:'Free your mind'})
CREATE
(Keanu)-[:ACTED_IN {roles:['Neo']}]->(TheMatrixReloaded),
(Carrie)-[:ACTED_IN {roles:['Trinity']}]->(TheMatrixReloaded),
(Laurence)-[:ACTED_IN {roles:['Morpheus']}]->(TheMatrixReloaded),
(Hugo)-[:ACTED_IN {roles:['Agent Smith']}]->(TheMatrixReloaded),
(LillyW)-[:DIRECTED]->(TheMatrixReloaded),
(LanaW)-[:DIRECTED]->(TheMatrixReloaded),
(JoelS)-[:PRODUCED]->(TheMatrixReloaded)

...
...
```

Create unique node property constraints to ensure that property values are unique for all nodes with a specific label. Adding the unique constraint, implicitly adds an index on that property.

```sql
CREATE CONSTRAINT FOR (n:Movie) REQUIRE (n.title) IS UNIQUE
CREATE CONSTRAINT FOR (n:Person) REQUIRE (n.name) IS UNIQUE
```

Create indexes on one or more properties for all nodes that have a given label. Indexes are used to increase search performance:

```sql
CREATE INDEX FOR (m:Movie) ON (m.released)
```

Find the actor named "Tom Hanks":

```sql
MATCH (tom:Person {name: "Tom Hanks"})
RETURN tom
```

Find the movie with title "Cloud Atlas":

```sql
MATCH (cloudAtlas:Movie {title: "Cloud Atlas"})
RETURN cloudAtlas
```

Find 10 people and return their names:

```sql
MATCH (people:Person)
RETURN people.name LIMIT 10
```

Find movies released in the 1990s and return their titles.

```sql
MATCH (nineties:Movie)
WHERE nineties.released >= 1990 AND nineties.released < 2000
RETURN nineties.title
```
