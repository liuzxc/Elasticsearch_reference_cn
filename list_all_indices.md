# 列出所有索引

以下命令可以列出所有的索引：

```shell
curl 'localhost:9200/_cat/indices?v'
```

响应：

```shell
curl 'localhost:9200/_cat/indices?v'
health index pri rep docs.count docs.deleted store.size pri.store.size
```

上面的响应表明集群中还没有索引。