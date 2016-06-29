# 集群健康度（cluster health)
让我们来做一个基本的健康检查，可以使用 `curl` 命令来实现，也可以用其它的工具去生成 HTTP/REST 调用。假设在同一节点上启动 Elasticsearch，并打开另一个命令行窗口。

为了检查集群健康度，我们将使用[_cat api](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html)。记住 HTTP 端口是 9200:

```shell
curl 'localhost:9200/_cat/health?v'
```

响应:

```shell
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign
1394735289 14:28:09  elasticsearch green           1         1      0   0    0    0        0
```

我们可以看到名叫 "elasticsearch" 集群处于 "green" 状态。

无论什么时候检查集群健康度，我们可能会得到绿（green），黄（yellow）和红（red）三种状态。绿色意味着集群是完全健康的，黄色意味着所有的数据可用但是副本尚未被分配，红色意味着一些数据因为某些原因还不可用。需要注意的是，即使是处于红色的状态，Elasticsearch 仍然具有部分功能（可用分片仍然可以响应搜索请求），但因为有数据丢失，所以你需要尽快修复问题。

从上面的响应内容中可以看出，由于还没有数据，因此我们已拥有1个节点0个分片。因为使用了默认的集群名称（elasticsearch），而且 Elasticsearch 使用单播网络去发现同机器上的其它节点，所以在你自己的计算机上可能启动了不止一个节点，这些节点都会加入到同一个集群当中。在这种情况下，上面的响应中显示的节点会不止一个。

我们也可以通过以下命令获取集群的节点列表：

```shell
curl 'localhost:9200/_cat/nodes?v'
```

响应：

```shell
curl 'localhost:9200/_cat/nodes?v'
host         ip        heap.percent ram.percent load node.role master name
mwubuntu1    127.0.1.1            8           4 0.00 d         *      New Goblin
```

我们可以看到一个叫 "New Goblin" 的节点，它是我们集群中的唯一节点。