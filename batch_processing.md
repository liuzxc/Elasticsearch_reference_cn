# 批量操作

出了可以索引，更新，删除单个的文档，Elasticsearch 还可以使用[_bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html) 进行批量操作。这个功能非常重要，它提供了一个非常有效的机制去尽可能快的执行多个操作并减少网络消耗。

下面的例子会展示用一个`bulk`操作索引两个文档（ID 1 - John Doe 和 ID 2 - Jane Doe）：

```sh
curl -XPOST 'localhost:9200/customer/external/_bulk?pretty' -d '
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
'
```

下面的例子会更新一个文档然后删除一个文档：

```sh
curl -XPOST 'localhost:9200/customer/external/_bulk?pretty' -d '
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
'
```
注意上面的删除操作，因为删除操作只要求 ID，所以没有带上源文档（`source document`）。

`bulk API` 是顺序的执行操作，如果单个操作失败，剩下的操作会继续进行。当`bulk API`返回的时候，它会提供每一步操作（与执行操作的顺序相同）的状态，以便与你检查某个特定的操作是否失败。




