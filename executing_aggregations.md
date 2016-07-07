# 执行聚合

聚合（Aggregations）提供了对数据进行分组和获取统计信息的能力，理解聚合的最简单方式是它大体上类似于
`SQL GROUP BY`和`SQL`聚合函数。在 Elasticsearch 当中，你可以通过执行查询返回命中结果，并同时返回
 聚合后的结果。这种特性是非常强大和高效的，你可以通过简洁的 API 去运行查询和聚合并同时获取结果，还可以
 避免信息传输带来的网络消耗。

 下面这个例子会把账户根据州分组，并返回总数逆序（默认值）排列的头 10(默认值）个州。

```sh
 curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state"
      }
    }
  }
}'
```

如果用`SQL`表示，上面的聚合类似于：

```sql
SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC

响应（显示部分）：

```json
  "hits" : {
    "total" : 1000,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_state" : {
      "buckets" : [ {
        "key" : "al",
        "doc_count" : 21
      }, {
        "key" : "tx",
        "doc_count" : 17
      }, {
        "key" : "id",
        "doc_count" : 15
      }, {
        "key" : "ma",
        "doc_count" : 15
      }, {
        "key" : "md",
        "doc_count" : 15
      }, {
        "key" : "pa",
        "doc_count" : 15
      }, {
        "key" : "dc",
        "doc_count" : 14
      }, {
        "key" : "me",
        "doc_count" : 14
      }, {
        "key" : "mo",
        "doc_count" : 14
      }, {
        "key" : "nd",
        "doc_count" : 14
      } ]
    }
  }
}
```
请注意：将`size`设置为 0 并不会显示搜索命中的结果，因为我们只想在响应中看到聚合的结果。

基于上面的例子，下面这个例子会根据州来计算账户的平均余额。

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state"
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}'
```
注意我们是如何把`average_balance`潜入到`group_by_state`聚合当中的，这是所有聚合的通用模式，
你可以随意的嵌套聚合去获取各种维度的汇总数据。

现在让我们逆序地去排序平均余额：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state",
        "order": {
          "average_balance": "desc"
        }
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}'
```
下面这个例子展示了如何根据年龄段分组，然后根据性别，最后获取每个年龄段，每种性别的平均账户余额：

```sh
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}'
```
还有许多其它的聚合特性，在这里我们不会细讲，如果你想要更深入的了解，可以参考[aggregations reference guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)。
