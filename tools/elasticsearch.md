# Elasticsearch

**Elasticsearch** is a distributed full-text search and analytics engine with an HTTP web interface and schema-free JSON documents.

Elasticsearch provides near real-time search and analytics for all types of data.

## Glossary

<https://www.elastic.co/guide/en/elastic-stack-glossary/current/terms.html>

## Commands

List all indexes:

```bash
curl http://localhost:9200/_aliases
# or:
curl http://localhost:9200/_cat/indices?v
# or:
curl http://localhost:9200/_status
```

List all documents in index:

```bash

curl http://localhost:9200:/smaple/_search
```

Query using URL parameters:

```bash
curl http://localhost:9200:/smaple/_search
```
