# 索引和查询文档
现在让我们放一些东西到 customer 索引，记住之前所讲到的，为了索引文档，我们应该告诉 Elasticsearch 使用索引中的哪一个类型（type）。

让我们索引一个简单的 customer 文档到 customer 索引，类型是 external，ID 是1:

```sh
curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
{
  "name": "John Doe"
}'
```

响应：

```sh
curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
{
  "name": "John Doe"
}'
{
  "_index" : "customer",
  "_type" : "external",
  "_id" : "1",
  "_version" : 1,
  "created" : true
}
```

从上面可以看出，我们已经成功的在 customer 索引和 external 类型中创建了 customer 文档，并且在索引的时候指定了一个内部 ID: 1。

值得注意的是，在索引文档的时候，Elasticsearch 并不要求你显示的创建一个索引。在上面的例子中，如果没有提前创建索引，Elasticsearch 将会自动创建一个名叫 customer 的索引。

获取刚才创建的文档：

```sh
curl -XGET 'localhost:9200/customer/external/1?pretty'
```
响应：

```sh
curl -XGET 'localhost:9200/customer/external/1?pretty'
{
  "_index" : "customer",
  "_type" : "external",
  "_id" : "1",
  "_version" : 1,
  "found" : true,
  "_source" : { "name": "John Doe" }
}
```
