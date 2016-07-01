# 修改您的数据

Elasticsearch 提供接近实时的数据处理和搜索能力，从数据被索引/更新/删除到数据出现在搜索结果中大概会有1秒钟左右的延迟，它与`SQL`及其它平台的不同之处在于：事物被提交之后，数据是立即可见的。

### 索引/替代文档

之前我们已经介绍过了如何索引一个文档：

```shell
curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
{
  "name": "John Doe"
}'
```
以上命令会索引指定的文档到`cutomer`这个 index，type 是`external`，ID 是1。如果我们用相同（或不同）的文档再次执行上面的命令，Elasticsearch 会在现有的 ID 为 1 的基础上重新索引新的文档：

```shell
curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
{
  "name": "Jane Doe"
}'
```

以上命令会把 ID 为 1 的文档 name 从 "John Doe" 变为 "Jane Doe"。如果换一种方式，我们使用不同的 ID，一个新的文档将会被索引，而已存在的文档会保持原样。

```shell
curl -XPUT 'localhost:9200/customer/external/2?pretty' -d '
{
  "name": "Jane Doe"
}'
```

以上命令会索引一个 ID 为 2 的新文档。

当索引文档的时候，ID 参数是可选的，如果不指定 ID，Elasticsearch 会自动生成一个随机 ID，然后用它去索引文档。无论是自己指定还是 Elasticsearch 自动为你生成 ID，它都会作为索引 API 的一部分被调用。

```shell
curl -XPOST 'localhost:9200/customer/external?pretty' -d '
{
  "name": "Jane Doe"
}'
```

注意上面这个例子，因为没有指定 ID，所以我们用了`POST`而不是`PUT`。