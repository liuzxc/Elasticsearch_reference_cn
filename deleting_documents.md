# 删除文档
删除一个文档是非常简单的。这个例子会展示如何删除之前`ID`为 2 的`customer`文档：

```sh
curl -XDELETE 'localhost:9200/customer/external/2?pretty'
```

`delete-by-query`插件可以删除匹配查询的多个文档。