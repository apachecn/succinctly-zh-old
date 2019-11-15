## 使用 cURL 更新数据

使用 Stargate 的 PUT 请求的语义与 HBase Shell 中的 put 命令非常相似。您在请求中指定了所需的最终状态，如果不存在，HBase 将执行其余的创建行或列，并设置该值。

您必须使用数据格式通过 Stargate 进行更新;否则你会得到 415 错误，“不支持的媒体类型。”使用 JSON，您可以从 GET 响应中镜像格式，因此您可以在单个请求中为多行发送多个单元格值。

URL 格式仍然需要行键和表，但是使用 PUT 时，URL 中的键将被忽略，以支持请求数据中的键。代码清单 51 显示了 PUT 请求，它更新了我的行中的两个单元格：

代码清单 51：使用 Stargate 更新单元格

```
$ curl -X PUT -H "Content-Type: application/json" -d '{
    "Row": [{
        "key": "ZWx0b258amVyaWNob3wyMDE1MTE=",
        "Cell": [{
            "column": "dDoxMTA2",
            "timestamp": 1447228701460,
            "$": "MTMw"
        }, {
            "column": "dDoxMTA3",
            "timestamp": 1447228695240,
            "$": "NjYw"
        }]
    }]
}' 'http://127.0.0.1:8080/access-logs/FAKE-KEY'

```

![](img/00012.jpeg)提示：Stargate 对于临时请求非常方便，但使用 Base64 可能很困难。我在博客上写了几篇简单的工具 [](https://blog.sixeyed.com/working-with-base64-and-stargate) 。

PUT 请求增加了我行中实际字符串的数值。通过 Stargate 无法使用原子递增计数器列的 incr 命令，因此如果需要增加值，则必须先读取它们，然后再 PUT 更新。

您可以使用 cURL 执行更多操作，例如发送 DELETE 请求以删除数据，以及创建扫描程序以获取多行，但语法变得很麻烦。在 Stargate 中使用 RESTful API 的包装器是一个更好的选择。