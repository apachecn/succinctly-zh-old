## 用 cURL 读取数据

代码清单 48 显示了根据 Stargate URL 的 GET 请求（cURL 中的默认动词）。响应与 HBase Shell 中的 list 命令相同：

代码 48：使用 cURL 列出表

```
$ curl http://127.0.0.1:8080
access-logs
social-usage

```

HTTP GET 请求相当于 HBase shell 中的 get 命令。您可以添加 Accept 标头以指定响应中所需的格式，但 Stargate 将提供的数据量有一些限制。

如果您尝试请求整个表格，例如 http://127.0.0.1:8080/access-logs ，您将收到错误响应，状态码为 405，表示不允许使用该方法。你不能在 HBase 中获取整个表，405 是 Stargate 的实现。

您可以通过在表名后添加行键来获取整行，但如果您的行格式中有任何不符合 HTTP 的字符，则需要在 URL 中转义它们。您也无法获得行的纯文本表示;您需要使用像 JSON 这样的以数据为中心的格式来生成请求。

代码清单 49 显示了从 Stargate 读取的整行。请注意，行键中的管道字符已作为％7C 进行转义，并且响应行键，列限定符和单元格值中的数据值均为 Base64 编码的字符串（我已应用）格式化为响应; Stargate 没有返回空格。

代码 49：用 cURL 读取一行

```
$ curl -H accept:application/json http://127.0.0.1:8080/access-logs/elton%7Cjericho%7C201511

{
    "Row": [{
        "key": "ZWx0b258amVyaWNob3wyMDE1MTE=",
        "Cell": [{
            "column": "dDoxMTA2",
            "timestamp": 1447228701460,
            "$": "MTIw"
        }, {
            "column": "dDoxMTA3",
            "timestamp": 1447228695240,
            "$": "NjUw"
        }]
    }]
}

```

响应是一个 JSON 对象，其中包含 Row 对象的数组。每行都有一个键字段和一个 Cell 对象数组。每个单元格都有一个列限定符，一个单元格值（存储在 $ 字段中）和一个时间戳。时间戳是 HBase 存储解释的唯一值;它们是长整数，在行更新时存储 UNIX 时间戳。

其他值是 Base64 字符串，这意味着您需要解码 GET 响应中的字段。表 6 显示了响应中一列的解码值：

| 领域 | 值 | 解码值 |
| Row.key | ZWx0b258amVyaWNob3wyMDE1MTE = | elton &#124; jericho &#124; 201511 |
| Cell.column | dDoxMTA2 | T：1106 |
| 细胞。$ | MTIw | 120 |

表 6：编码和解码的 Stargate 值

请注意，单元格中的列值是全名（列族加限定符，用冒号分隔），数值单元格值实际上是一个字符串。

您还可以从 Stargate 连续获取单列族（URL 格式为 / {table} / {row-key} / {column-family} ）或单个单元格值。单元格，您可以用纯文本格式获取它们，Stargate 将解码响应中的值，如**错误！未找到参考源。**：

代码清单 50：获取单个单元格值

```
$ curl http://127.0.0.1:8080/access-logs/elton%7Cjericho%7C201511/t:1106
120

```