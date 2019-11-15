## 使用 Python 中的扫描仪

表对象有 scan（）方法在区域服务器上创建扫描程序，您可以在客户端上迭代。您可以像 HBase Shell 一样使用 scan ，传递开始和停止行来定义边界，如代码清单 42 所示：

代码清单 42：使用 HappyBase 扫描行

```
>>> access_logs = connection.table('access-logs')
>>> scanner = access_logs.scan('elton|jericho|201510', 'elton|jericho|x')
>>> for key, data in scanner:
...     print key, data
...
elton|jericho|201510 {'t:2908': '80'}
elton|jericho|201511 {'t:1106': '120', 't:1107': '650'}

```

scan（）方法有一些友好的补充。您可以传递行键前缀而不是开始和停止行，HappyBase 为您设置边界;您还可以传递列族名称或列名称列表以限制响应中的数据，如代码清单 43 所示：

代码清单 43：使用 HappyBase 按前缀扫描行

```
>>> scanner = access_logs.scan(row_prefix='elton|jericho|', columns=['t:1106'])
>>> for key, data in scanner:
...     print key, data
...
elton|jericho|201511 {'t:1106': '120'}

```

Scan 返回一个可迭代对象，您可以将其作为单个结果集循环，尽管 HappyBase 实际上会从 Thrift 批量读取结果。您可以指定 batch_size 参数来调整扫描仪的读数;这默认为 1,000，这是一个合理的假设，支持大批量多次读取。

如果您使用宽表或大单元格大小，则可能需要减小批量大小以提高整体性能。如果从多行读取小单元格值，则批量较大可能更好。