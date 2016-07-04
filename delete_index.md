# 删除索引

现在删除刚刚创建的索引并列出所有的索引：

```sh
curl -XDELETE 'localhost:9200/customer?pretty'
curl 'localhost:9200/_cat/indices?v'
```
响应：

```sh
curl -XDELETE 'localhost:9200/customer?pretty'
{
  "acknowledged" : true
}
curl 'localhost:9200/_cat/indices?v'
health index pri rep docs.count docs.deleted store.size pri.store.size
```

索引已经被成功的删除，我们又回到了集群的初始状态。

在继续学习之前，让我们回顾下之前学到的 API 命令：

```sh
curl -XPUT 'localhost:9200/customer'
curl -XPUT 'localhost:9200/customer/external/1' -d '
{
  "name": "John Doe"
}'
curl 'localhost:9200/customer/external/1'
curl -XDELETE 'localhost:9200/customer'
```

如果细心的学习这些命令，我们会发现在 Elasticsearch 中访问数据的一种模式，这种模式可以总结为：

```
curl -X<REST Verb> <Node>:<Port>/<Index>/<Type>/<ID>
```

这种 REST 访问模式普遍存在所有的 API 命令中，如果你记住它，对于掌握 Elasticsearch 会是个良好的开端。

