# 更新文档
除了可以更新和替换一个文档外，还可以更新文档。需要注意的是，在 Elasticsearch 内部，它实际上并没有去真的更新文档，而是先删除文档，再索引一个新文档。

这个例子会展示如何更新之前的文档（ID为1）的 name 字段为 "Jane Doe"：

```sh
curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
{
  "doc": { "name": "Jane Doe" }
}'
```

接着下面这个例子会展示如何把之前的文档（ID为1）的`name` 字段修改为 "Jane Doe" 并且再添加一个`age`字段：

```sh
curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
{
  "doc": { "name": "Jane Doe", "age": 20 }
}'
```

文档更新也可以通过简单的脚本来实现。主要注意的是，像下面这种动态脚本（dynamic scripts）在`1.4.3`这个版本中是默认关闭的，从这个地方[scripting docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting.html)可以了解更多细节。下面的例子使用脚本给年龄值加 5:

```sh
curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
{
  "script" : "ctx._source.age += 5"
}'
```

在上面的例子中，`ctx._source` 表明当前的源文档被更新。

这种写法需要注意的是，更新只会作用于单个文档，在未来，Elasticsearch 可能会提供更具查询条件更新多个文档的功能（就像`SQL`的`update where`语句）。


