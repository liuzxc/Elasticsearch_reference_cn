# 设置

这一节的内容包括了如何设置和运行 elasticsearch，如果你还没有下载，请参考[安装](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup.html#setup-installation)文档。

> Elasticsearch 也可以使用 apt 和 yum 安装，参见 [Repositories](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-repositories.html)。

# 平台支持

官方的支持的操作系统和 JVMs 可以在这里看到：[Support Matrix](https://www.elastic.co/support/matrix)。
Elasticsearch 已经在这些平台测试过了，但也有可能在其它的平台运行。

# 安装

在下载和解压最新的版本之后，可以这样启动 elasticsearch:

```sh
$ bin/elasticsearch
```

在类 unix 操作系统下，这个命令将在前台启动进程

# 后台运行

可以添加 -d 参数让 elasticsearch 在后台运行：

```sh
$ bin/elasticsearch -d
```

# PID

在启动的时候，elasticsearch 进程可以把它的 PID 写到指定的文件，让之后关闭进程更容易：

```sh
$ bin/elasticsearch -d -p pid  # 把 PID 写入一个叫 pid 的文件
$ kill `cat pid`  # kill 命令发送一个 TERM 信号给文件中的 PID
```

> 为 [Linux](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-service.html) 和 [Windows](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-service-win.html) 提供的启动脚本会帮助你启动和关闭 elasticsearch 进程。

在使用 elasticsearch shell 脚本的时候会有一些新增的功能，首先，正如之前提到的，它很容易在前台或者后台运行进程；
其次可以直接给脚本传递 -D 或者一些配置参数。例如：

```sh
$ bin/elasticsearch -Des.index.refresh_interval=5s --node.name=my-node
```

# Java(JVM)版本

Elasticsearch 是由 Java 构建，要求至少是 Java7 的版本，只支持 Oracle 的 Java 和 OpenJDK。所有的 Elasticsearch 的节点和客户端应该使用相同版本的 JVM。

我们推荐安装 Java 8 update 20 或更新， Java 7 update 55 或更新，之前的 Java7 版本有已知的 bug 会导致索引失败和数据丢失。如果使用了已知的不可用的 Java 版本，Elasticsearch 会拒绝启动。

Java 的版本可以通过配置 `JAVA_HOME` 环境变量来配置。
