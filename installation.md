# 安装

Elasticsearch 要求 Java 版本至少是 Java 7。在撰写本文时，推荐使用 Oracle JDK version 1.8.0_73。Java 安装因平台不同而有所不同，所以我们不会深入这些细节。Oracle 推荐的安装文档可以在 [Oracle’s website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html) 找到。在安装 Elasticsearch 之前，请首先检查 Java 版本：

```sh
java -version
echo $JAVA_HOME
```

一旦 JAVA 安装完毕，就可以下载和安装 Elasticsearch。二进制文件可以从[www.elastic.co/downloads](https://www.elastic.co/downloads) 下载所有的 releases。针对每一个 release，你可以选择 `zip` 或 `tar` 存档文件，或者是 `DEB`，`RPM` package。为简单起见，我们使用 tar 文件。

按照下面的命令下载 `Elasticsearch 2.3.3`（Windows 用户应该选择 `zip` package）

```sh
curl -L -O https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.3.3/elasticsearch-2.3.3.tar.gz
```

然后解压：

```sh
tar -xvf elasticsearch-2.3.3.tar.gz
```

该命令会在当前目录创建一系列的文件和文件夹，进入到 `bin` 目录：

```sh
cd elasticsearch-2.3.3/bin
```

然后启动节点和单个集群：

```sh
./elasticsearch
```

如果一切顺利，你可以看到如下的一系列消息：

```log
./elasticsearch
[2014-03-13 13:42:17,218][INFO ][node           ] [New Goblin] version[2.3.3], pid[2085], build[5c03844/2014-02-25T15:52:53Z]
[2014-03-13 13:42:17,219][INFO ][node           ] [New Goblin] initializing ...
[2014-03-13 13:42:17,223][INFO ][plugins        ] [New Goblin] loaded [], sites []
[2014-03-13 13:42:19,831][INFO ][node           ] [New Goblin] initialized
[2014-03-13 13:42:19,832][INFO ][node           ] [New Goblin] starting ...
[2014-03-13 13:42:19,958][INFO ][transport      ] [New Goblin] bound_address {inet[/0:0:0:0:0:0:0:0:9300]}, publish_address {inet[/192.168.8.112:9300]}
[2014-03-13 13:42:23,030][INFO ][cluster.service] [New Goblin] new_master [New Goblin][rWMtGj3dQouz2r6ZFL9v4g][mwubuntu1][inet[/192.168.8.112:9300]], reason: zen-disco-join (elected_as_master)
[2014-03-13 13:42:23,100][INFO ][discovery      ] [New Goblin] elasticsearch/rWMtGj3dQouz2r6ZFL9v4g
[2014-03-13 13:42:23,125][INFO ][http           ] [New Goblin] bound_address {inet[/0:0:0:0:0:0:0:0:9200]}, publish_address {inet[/192.168.8.112:9200]}
[2014-03-13 13:42:23,629][INFO ][gateway        ] [New Goblin] recovered [1] indices into cluster_state
[2014-03-13 13:42:23,630][INFO ][node           ] [New Goblin] started
```

没有太多的细节，可以看到叫做 "New Goblin"（在你的机器上可能有所不同）的节点被启动，并且被集群选择为主节点。不用担心主节点意味着什么，重要的是我们在集群内启动了一个节点。

正如我们之前所提到的，我们可以覆盖集群或节点的名称。可以通过以下命令实现：

```sh
./elasticsearch --cluster.name my_cluster_name --node.name my_node_name
```

注意 HTTP 地址(192.168.8.112)和端口(9200)信息，默认情况下，Elasticsearch 使用 9200 端口去提供其 REST API 访问权限。如果必要，这个端口是可配置的。