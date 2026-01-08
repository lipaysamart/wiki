---
weight: 1110
title: "ElasticSearch 集群缩容"
description: ""
icon: "article"
date: "2026-01-07T18:19:58-08:00"
lastmod: "2026-01-07T18:19:58-08:00"
draft: true
toc: true
tags: ["elasticsearch"]
categories: ["database"]
---

## Prerequisite

- curl
- elasticsearch

---

## Producer

- 检查集群信息

{{< alert context="info" text="确认剩余的节点有足够的磁盘空间承载迁过来的数据" />}}

```sh
curl -s -XGET "http://localhost:9200/_cat/allocation?v"
```

- 迁移数据并在路由策略中排除节点

```sh
curl -X PUT "localhost:9200/_cluster/settings" -H 'Content-Type: application/json' -d'
{
  "transient" : {
    "cluster.routing.allocation.exclude._name" : "elasticsearch-master-2"
  }
}'
```

{{< alert context="info" text="排除多个节点，可以使用逗号分隔：elasticsearch-master-2,elasticsearch-master-3" />}}

- 确认数据清空, `shards` 数量变为 0

```sh
curl -s -XGET "http://localhost:9200/_cat/allocation?v"
```

- 下线节点
- 查看集群健康状态

```sh
curl -s -XGET "http://localhost:9200/_cluster/health
```

---

## Reference
