# Aggregation
## Merics Aggregations
- min
- max
- avg
- sum
- stats
  - contain min, max, avg, sum, count
- percentiles(25, 50, 75)
  - 50% equal to median
- cardinality
  - unique count

### sum
```
GET /logs_server*/_search
{
    "size": 0,  // This option can fetch a only aggs value
    "query": {  // Query clause can limit the scope for aggs
        ...
    },
    "aggs": {
        "total_sum_bytes": {
            "sum": {
                "field": "response_size"
            }
        }
    }
}
```

### percentiles
```
GET /logs_server*/_search
{
    "size": 0,
    "query": {
        ...
    },
    "aggs": {
        "runtime_quartiiles": {
            "field": "runtime_ms",
            "percents": [
                25,
                50, // = median
                75
            ]
        }
    }
}
```

### cardinality
```
GET /logs_server*/_search
{
    "size": 0,
    "aggs": {
        "number-of_countries": {
            "cardinality": {
                "field": "geoip.country_name.keyword"
            }
        }
    }
}
```

## Bucket Aggregations
- date_histogram
- histogram
- range
- terms
- bucket sorting

### date_histogram
**usecase:** How many requests reach our system every day?
**points:**
- Be careful wiith the interval. 
  - Creting too many buckets can harm the cluster.
```
GET /logs_server*/_search
{
    "size"; 0,
    "aggs": {
        "logs_by_day": {
            "date_histogram": {
                "field": "@timestamp",
                "interval": "day"
            }
        }
    }
}
```

### histogram
```
GET /logs_server*/_search
{
    "size": 0,
    "aggs": {
        "runtime_histogram": {
            "histogram": {
                "field": "runtime_ms",
                "interval"; 100
            }
        }
    }
}
```

### range
**usecase:** How many requests took between 0-200, 200-500, 500-* ms?
```
GET /logs_server*/_search
{
    "size": 0,
    "aggs": {
        "runtime_breakdown": {
            "range": {
                "field": "runtime_ms",
                "ranges": [
                    {
                        "from": 0,
                        "to": 200
                    },
                    {
                        "from": 200,
                        "to": 500
                    },
                    {
                        "from": 500
                    }
                ]
            }
        }
    }
}
```

### terms
**usecase:** What are the top 5 countries that visited our blogs?
**points:**
- size: number of buckets to create(default is 10)
```
GET /logs_server*/_search
{
    "size": 0,
    "aggs": {
        "country_name_terms": {
            "terms": {
                "field": "geoip.country_name.keyword",
                "size": 5
            }
        }
    }
}
```

### bucket sorting
- _count: sorts by doc_count(default in terms)
- _key: sorts by laphabetically(default in histogram and date_histogram)
```
GET /logs_server*/_search
{
    "size":0,
    "aggs": {
        "logs_by_day": {
            "date_histogram": {
                "field": "@timestamp",
                "interval": "day",
                "order": {
                    "_key": "desc"
                }
            }
        }
    }
}
```

## Combining Aggregations
### A single Metric per Bucket
```
GET /logs_server*/_search
{
    "size": 0,
    "aggs": {
        "requests_per_day": {
            "date_histogram": {
                "field": "@timestamp",
                "interval": "day"
            },
            "aggs": {
                "daily_number_of_bytes": {
                    "sum": {
                        "field": "response_size"
                    }
                }
            }
        }
    }
}
```

### Multiple Metrics per Bucket
```
GET /logs_server*/_search
{
    "requests_per_day": {
        "date_histogram": {
            "field": "@timestamp",
            "interval": "day"
        },
        "aggs": {
            "daily_number_of_bytes": {
                "sum": {
                    "field": "response_size"
                }
            },
            "median_runtime": {
                "percentiles": {
                    "field": "runtime_ms",
                    "percents": [50]
                }
            }
        }
    }
}
```