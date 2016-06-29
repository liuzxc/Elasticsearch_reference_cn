# 创建索引

现在我们创建一个名叫 "customer" 的索引，并且再次列出所有的索引：

```shell
curl -XPUT 'localhost:9200/customer?pretty'
curl 'localhost:9200/_cat/indices?v'
```

第一个命令使用 PUT 创建了名为 "customer" 的索引，添加 pretty 在命令的最后可以把响应以 JSON 的格式输出：

```shell
curl -XPUT 'localhost:9200/customer?pretty'
{
  "acknowledged" : true
}

curl 'localhost:9200/_cat/indices?v'
health index    pri rep docs.count docs.deleted store.size pri.store.size
yellow customer   5   1          0            0       495b           495b
```

第二个命令告诉我们现在有一个名叫 customer 的索引，这个索引包涵5个主分片和一个副本，但没有包涵文档。

你可能会注意到 customer 索引有个黄色的健康标记，之前说过黄色意味着一些副本还没有被分配，因为 Elasticsearch 默认为索引创建一个副本，但是由于目前只有一个节点，所以副本还不能被分配（副本和它的原始分片不能在同一节点上），直到有另外的节点加入集群。一旦副本被分配到另外一个节点，集群的健康状态将会变绿。