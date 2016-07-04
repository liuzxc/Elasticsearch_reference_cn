# 探索您的数据

### 样本数据集
我们已经学习了一些基础知识，现在让我们来处理一些更为现实的数据集。我准备了一个虚构的客户银行账户 JSON 文档，每个文档有如下的结构：

```json
{
    "account_number": 0,
    "balance": 16623,
    "firstname": "Bradshaw",
    "lastname": "Mckenzie",
    "age": 29,
    "gender": "F",
    "address": "244 Columbus Place",
    "employer": "Euron",
    "email": "bradshawmckenzie@euron.com",
    "city": "Hobucken",
    "state": "CO"
}
```
我是通过网站[www.json-generator.com/](http://www.json-generator.com/)生成这些数据的，所以请忽略这些数据的实际意义，它们都是随机生成的。

你可以从[这里](https://github.com/bly2k/files/blob/master/accounts.zip?raw=true)下载样本数据（accounts.json），解压文件到当前目录并按照以下命令把数据装载到集群：

```shell
curl -XPOST 'localhost:9200/bank/account/_bulk?pretty' --data-binary "@accounts.json"
curl 'localhost:9200/_cat/indices?v'
```