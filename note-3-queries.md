# CRUD Operations

# Combining Searches
## Bool Query
**usecase:** find blogs about *"Logstash"* in the *"Engineering"* category
```
GET /index/_search
{
    "query": {
        "bool": {
            "must": [
                {}
            ],
            "must_not": [
                {}
            ],
            "should": [
                {}
            ],
            "filter"; [
                {}
            ]
        }
    }
}
```

### The must Clause
```
GET /index/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "match": {
                        "content": "logstash"
                    }
                },
                {
                    "match": {
                        "category": "engineering"
                    }
                }
            ]
        }
    }
}
```

### The must_not Clause
```
GET /index/_search
{
    "query": {
        "bool": {
            "must": {
                "match": {
                    "content": "logstash"
                }
            },
            "must_not": {
                "match": {
                    "category": "releases"
                }
            }
        }
    }
}
```

### The should Clause
```
GET /index/_search
{
    "query": {
        "bool": {
            "must": {
                "match_phrase": {
                    "content": "elastic stack"
                }
            },
            "should": {
                "match_phrase": {
                    "author": "shay banon"
                }
            }
        }
    }
}
```

### The filter Clause
```
GET /index/_search
{
    "query": {
        "bool": {
            "must": {
                "match_phrase": {
                    "content": "elastic stack"
                }
            },
            "filter": {
                "match": {
                    "category": "engineering"
                }
            }
        }
    }
}
```

## Pagination
from default to 0
Follow example can fetch the page 2(10 to 20)
```
GET /blogs/_search
{
    "from": 10,
    "size": 10,
    "query": {
        "match": {
            "content": "elasticsearch"
        }
    }
}
```

## Highlighting and Changing the Tags
```
GET /blogs/_search
{
    "query": {
        "match_phrase": {
            "title": "kibana"
        }
    },
    "highlight": {
        "fields": {
            "title": {}
        },
        "pre_tags": ["<es-hit>"],
        "post_tags": ["</es-hit>"]
    }
}
```
