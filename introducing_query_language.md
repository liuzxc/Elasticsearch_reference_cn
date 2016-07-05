# 查询语言（query language）介绍

Elasticsearch 提供一种被称作 [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html) 的 JSON 风格的领域专属语言去执行查询。这种查询语言乍看之下令人生畏，但是学习它的最好方式是通过一些实例。

回顾之前的例子，我们执行了这样一个查询：

```sh
{
  "query": { "match_all": {} }
}
```
`query`部分告诉了我们查询的定义，`match_all`告诉我们想要运行的查询类型，`match_all`查询简单的搜索指定索引下的所有文档。

除了`query`参数以外，我们还可以通过传递其它的参数去影响查询的结果。下面的例子使用`match_all`查询并只返回第一个文档：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "size": 1
}'
```

请注意：如果不指定`size`，默认值是 10。

下面的例子使用`match_all`查询并返回 11-20 的文档：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "from": 10,
  "size": 10
}'
```
`from`指定了从哪一个文档开始的下标，而`size`参数指定了返回从这个下标开始后的多少个文档，这个特性对于分页查询是非常有用的。请注意：如果`from`没有指定，默认是 0。

下面的例子匹配一个`match_all`查询，根据账户余额降序排序结果，并且返回头 10（默认 size） 个文档：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "sort": { "balance": { "order": "desc" } }
}'
```