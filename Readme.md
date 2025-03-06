# Elasticsearch Commands

This document contains various Elasticsearch commands for working with indices, documents, mappings, queries, and more.

## Table of Contents

* Index Management
* Shard Information
* Cluster Health
* Document Operations
* Querying Data
* Updating Documents
* Mapping &amp; Schema
* Reindexing
* Index Templates
* Aliases

---

## Index Management

### Show All Indices

```sh
GET _cat/indices?v
```

### Create an Index

```sh
PUT /fruits
```

### Delete an Index

```sh
DELETE students
```

### Create Index with Settings

```sh
PUT students
{
  "settings": {
    "number_of_replicas": 2,
    "number_of_shards" : 3
  }
}
```

---

## Shard Information

### List Shards

```sh
GET _cat/shards?v
```

---

## Cluster Health

### Show Cluster Health

```sh
GET /_cluster/health
```

---

## Document Operations

### Add a Document

```sh
POST students/_doc
{
  "name": "sohil",
  "age": 25
}
```

### Add or Replace a Document with Specific ID

```sh
PUT students/_doc/NcI-KJUBOmE4S1_NgR8w
{
  "name": "sohil1",
  "age": 25
}
```

### Delete a Document

```sh
DELETE students/_doc/NcI-KJUBOmE4S1_NgR8w
```

---

## Querying Data

### Fetch All Documents

```sh
GET students/_search
{
  "query": {
    "match_all": {}
  }
}
```

---

## Updating Documents

### Update a Document by ID

```sh
POST students/_update/XMJHKJUBOmE4S1_NAR_W
{
  "doc": {
    "age": 25,
    "lastname": "lalakiya"
  }
}
```

### Update Document Using a Script

```sh
POST students/_update/XMJHKJUBOmE4S1_NAR_W
{
  "script": {
    "source": "ctx._source.age = 5 + params.new_age",
    "params": {
      "new_age": 10
    }
  }
}
```

### Upsert Document When Condition Fails

```sh
POST students/_update/NcI-KJUBOmE4S1_NgR8
{
  "script": {
    "source": """
    if(ctx._source.age > 20){
      ctx._source.age--
    }else{
      ctx.op = 'noop'
    }
    """
  },
  "upsert": {
    "name": "upserted document",
    "age": 23
  }
}
```

### Bulk API (Insert, Update, Delete Multiple Documents)

```sh
POST students/_bulk
{"index": {}}
{"name": "vishal", "age": 21}
{"create": {}}
{"name": "vishal", "age": 21}
{"update": {"_id": "u-J1NpUBOKPMO7KI-9hz"}}
{"doc": {"age": 25}}
{"delete": {"_id": "wOJ2NpUBOKPMO7KIoNjV"}}
```

---

## Mapping & Schema

### Get Mapping of an Index

```sh
GET students/_mapping
```

### Create Index with Mapping and Dynamic Settings

```sh
PUT movies
{
  "mappings": {
    "dynamic": false,
    "properties": {
      "name": { "type": "text" },
      "rating": { "type": "float" },
      "budget": { "type": "float" },
      "directors": {
        "properties": {
          "name": { "type": "text" },
          "email": { "type": "keyword" }
        }
      },
      "actors": {
        "type": "nested",
        "properties": {
          "name": { "type": "text" },
          "age": { "type": "integer" }
        }
      }
    }
  }
}
```

---

## Reindexing

### Reindex Data from One Index to Another

```sh
POST _reindex
{
  "source": {
    "index": "students"
  },
  "dest": {
    "index": "students_new"
  },
  "script": {
    "source": "ctx._source.id = ctx._source.name + ctx._source.age"
  }
}
```

---

## Index Templates

### Create an Index Template

```sh
PUT _index_template/student_template
{
  "index_patterns": ["student_class_*"]
  "template": {
    "settings": {
      "number_of_shards": 2,
      "number_of_replicas": 2
    },
    "mappings": {
      "properties": {
        "name": { "type": "text" },
        "email": { "type": "keyword" },
        "dob": { "type": "date" }
      }
    }
  }
}
```

### Delete an Index Template

```sh
DELETE _index_template/student_template
```

---

## Aliases

### Create and Manage Index Aliases

```sh
POST _aliases
{
  "actions": [
    { "remove": { "index": "customer_data", "alias": "customers" } },
    { "add": { "index": "customer_data", "alias": "customers" } },
    { "add": { "index": "customer_data2", "alias": "customers" } }
  ]
}
```

### Search Using Alias

```sh
GET customers/_search
{
  "query": {
    "match_all": {}
  }
}
```

---

## Conclusion

This document provides various Elasticsearch operations, from index management to querying and updating documents. Modify and extend these commands based on your needs.
