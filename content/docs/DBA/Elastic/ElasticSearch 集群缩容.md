---
weight: 1110
title: "ElasticSearch Cluster Scale-In"
description: ""
icon: "article"
date: "2026-01-07T18:19:58-08:00"
lastmod: "2026-01-07T18:19:58-08:00"
draft: true
toc: true
tags: ["elasticsearch"]
# categories: ["elastic"]
---

## Prerequisite

- curl
- elasticsearch

---

## Procedure

- Check cluster information

{{< alert context="info" text="Make sure the remaining nodes have enough disk space to hold the migrated data." />}}

```sh
curl -s -XGET "http://localhost:9200/_cat/allocation?v"
```

- Migrate data and exclude the node in the routing policy

```sh
curl -X PUT "localhost:9200/_cluster/settings" -H 'Content-Type: application/json' -d'
{
  "transient" : {
    "cluster.routing.allocation.exclude._name" : "elasticsearch-master-2"
  }
}'
```

{{< alert context="info" text="To exclude multiple nodes, separate them with commas: elasticsearch-master-2,elasticsearch-master-3" />}}

- Confirm the data has been cleared and the number of `shards` becomes 0

```sh
curl -s -XGET "http://localhost:9200/_cat/allocation?v"
```

- Decommission the node
- Check cluster health status

```sh
curl -s -XGET "http://localhost:9200/_cluster/health
```

---

## Reference
