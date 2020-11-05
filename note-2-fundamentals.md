# Fundamentals
## What is the Elasticsearch?
- Elasticsearch was developed by Shay Banon in 2010
  - It was completely rewrote from Compass
  - These are based on Apache Lucene
- Two main objectives
  - It is distributed and scales horizontally
  - Easily used by any other programming language by providing REST APIs
- Elastic Stack
  - Elasticsearch
    - core product to store, search, analyze & manage
  - Logstash
    - server-side data processing pipeline
  - Kibana
    - analytics and visualization platform
  - Beats
    - single-purpose data shippers

## Elasticsearch Directories
- bin
  - elasticsearch
  - elasticsearch-plugin
- config
  - elasticsearch.yml
  - jvm.options
  - log4j2.properties
- data
  - indices data files
  - shard allocated data files
- jdk
  - bundled OpenJDK
- lib
  - the Java JAR files
- logs
  - log files of the cluster
- modules
  - Elasticsearch modules
- plugins
  - Elasticsearch plugins

## Config files
- elasticsearch.yml
  - settings node and cluster settings
- jvm.options
  - size of heap space
- log4j2.properties
  - log settings
- kibana.yml
  - settings to connect to elasticsearch nodes
- Two ways to define configuration properties
  - config file
  - command line option

## Terms
- A node is an instance of Elasticsearch
- A cluster is one or multiple nodes working together in a distributed manner