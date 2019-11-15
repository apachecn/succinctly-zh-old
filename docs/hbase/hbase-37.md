## Python 中的扫描仪和过滤器

Thrift 支持具有在 Region Server 上运行的过滤器的扫描程序。扫描程序从提供的行键边界有效地读取行，并且过滤器仅提取您想要返回的行或列。

HappyBase 允许您在 scan（）方法中从客户端创建过滤扫描仪。这是 HappyBase 不会抽象复杂性的一个领域，您必须根据 Thrift API 过滤语言将过滤器构造为字符串。

过滤字符串的一般格式是 {过滤器名称}（{arguments}）。代码清单 44 显示了一个过滤器，该过滤器仅返回扫描中具有以前缀“11”开头的列中的值的行：

代码清单 44：使用 HappyBase 扫描和过滤行

```
>>> access_logs = connection.table('access-logs')
>>> scanner = access_logs.scan('elton|jericho|201510', 'elton|jericho|x', filter="ColumnPrefixFilter('11')")
>>> for key, data in scanner:
...     print key, data
...
elton|jericho|201511 {'t:1106': '120', 't:1107': '650'}

```

![](img/00012.jpeg)提示：在 HBase 在线文档中详细记录了 Thrift API，并提供了您需要提供的可用过滤器和参数 [](http://hbase.apache.org/book.html#individualfiltersyntax) 。