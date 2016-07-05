# 执行搜索

我们已经学习了一些基本的搜索参数，接下来让我们更深入的了解一下`Query DSL`。首先让我们看一下返回的文档字段，默认情况下，整个 JSON 文档以 `source`（`search hits`中的`_source`字段）字段作为搜索结果的一部分被返回。如果我们不想返回整个源文档（`source document`），我们可以只请求返回源文档中的某一些字段。

下面这个例子展示了如何只返回`account_number`和`balance`这两个字段：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}'
```
上面的例子缩减了`_source`字段，它仍然返回了一个叫`_source`的字段，但是这歌字段只包含了`account_number`和`balance`。如果你了解`SQL`，这个例子类似于`SQL`里面的`SELECT FROM`语句。

现在让我们来看看查询部分，之前我们已经学习了如何使用 match_all 去匹配所有的文档，现在我们来学习一个被称作 [match query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html) 的查询，可以认为它是一种
基本的字段搜索查询（例如：搜索特定的字段或一组字段）。下面这个例子将返回账户号是 20 的文档：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match": { "account_number": 20 } }
}'
```
下面的例子将返回所有账户地址中包涵 "mill" 的词条：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match": { "address": "mill" } }
}'
```

下面这个例子将返回所有账户地址中包涵 "mill" 或 "lane" 的词条：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match": { "address": "mill lane" } }
}'
```

下面这个例子是`match`的一个变种（`match_phrase`），它返回所有账户地址中包涵 "mill lane" 的词干：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_phrase": { "address": "mill lane" } }
}'
```

接下来我们来介绍一下布尔查询（[bool(ean) query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html)），布尔查询允许我们使用布尔逻辑组合较小的查询到更大的查询中。下面这个例子组合了两个`match`查询，返回所有地址中包涵 "mill" 和 "lane" 的账户：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'
```
上面的例子中，`bool must`声明表示如果一个文档要匹配，所有的查询条件必须为真。

相比之下，下面这个例子也组合了两个`match`查询，返回地址中包括 "mill" 或 "lane" 的所有账户：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { 
    "bool": {
      "should": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'
```
`bool should`声明表示如果一个文档要匹配，那么至少有一个查询条件需要匹配。

下面这个例子也组合了两个`match`查询，返回地址中既不包涵 "mill" 也不包涵 "lane" 的所有账户：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'
```
`bool must_not` 声明表示如果一个文档要匹配，查询条件都不能为真。

我们在一个`bool`查询中结合了`must`，`should`，`must_not`声明，接下来我们把`bool`查询和其它的`bool`查询结合起来，形成多重的`bool`逻辑。下面这个例子将返回所有 40 岁的人但是没有生活在 ID（aho，应该是个地名） 的账户信息：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}'
```