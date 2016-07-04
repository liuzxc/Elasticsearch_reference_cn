# 搜索 API

现在我们可以尝试一些简单的搜索操作。运行搜索有两种基本方式：一种是通过 [REST request URI](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-uri-request.html) 发送搜索参数，另外一种是通过 [REST request body](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html) 发送搜索参数。第二种方法可以更富有表现力，你可以通过更自由的 JSON 格式去定义你的搜索。我们会用一个例子去展示如何使用第一种方法，对于教程的其余部分，我们会专门使用第二种方法。

搜索的 REST API 从`_search`端点开始，这个例子将返回`bank`索引中的所有文档：

```sh
curl 'localhost:9200/bank/_search?q=*&pretty'
```
让我们分析一下这个搜索调用：我们搜索`bank`索引，通过`q=*`参数告诉 Elasticsearch 去匹配索引中的所有文档，`pretty`参数只是告诉 Elasticsearch 用漂亮的格式返回 JSON 结果。

响应（只显示部分结果）：
```sh
curl 'localhost:9200/bank/_search?q=*&pretty'
{
  "took" : 63,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "1",
      "_score" : 1.0, "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "6",
      "_score" : 1.0, "_source" : {"account_number":6,"balance":5686,"firstname":"Hattie","lastname":"Bond","age":36,"gender":"M","address":"671 Bristol Street","employer":"Netagy","email":"hattiebond@netagy.com","city":"Dante","state":"TN"}
    }, {
      "_index" : "bank",
      "_type" : "account",
```
根据响应，我们可以看到以下几个部分：
* `took` - Elasticsearch 执行搜索的时间（以毫秒为单位）
* `timed_out` － 告诉我们搜索是否超时
* `_shards` － 告诉我们多少分片被搜索，包括成功／失败的搜索分片的计数
* `hits` － 搜索结果
* `hits.total` － 匹配搜索标准的文档数量
* `hits.hits` － 实际的搜索结果数组（默认前10个文档）
* `_score`和`max_score` － 现在忽略这些字段

现在使用请求体方法实现和上面一样的搜索：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} }
}'
```
不同之处在于，我们`POST`一个 JSON 样式的查询请求体到`_search API`，而不是在`URL`中添加`q=*`参数，我们将在下一节讨论 JSON 查询。

响应（部分显示）：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} }
}'
{
  "took" : 26,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "1",
      "_score" : 1.0, "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "6",
      "_score" : 1.0, "_source" : {"account_number":6,"balance":5686,"firstname":"Hattie","lastname":"Bond","age":36,"gender":"M","address":"671 Bristol Street","employer":"Netagy","email":"hattiebond@netagy.com","city":"Dante","state":"TN"}
    }, {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "13",
```
一旦你获得了搜索结果，Elasticsearch 已经完全的完成了请求，它不会维护任何类型的服务端资源和打开游标到你的结果，理解这一点是非常重要的。这与 SQL 这样的平台形成了鲜明的对比，一开始你也许只想获取部分的查询结果，然后你想持续不断的获取结果，你可以通过有状态的服务端游标去获取剩下的数据。（注：其实我还不太明白这句话的意思）。