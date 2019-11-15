## 从 Python 编写数据

HappyBase 表对象上的 put（）方法与 HBase Shell 中的 put 命令非常相似，它采用行键，列名和值。但是使用 HappyBase，您可以使用单个语句更新和插入多个单元格值，并传递 _ 键的字典：值 _ 对，如代码清单 45 所示：

代码 45：使用 HappyBase 更新数据

```
>>> access_logs.put('elton|jericho|201511', {'t:1309':'400', 't:1310':'200'})
>>> print access_logs.row('elton|jericho|201511', ['t:1309', 't:1310'])
{'t:1310': '200', 't:1309': '400'}

```

put（）方法仅限于一行，但 HappyBase 为批量更新提供了一种有用的机制。这是 HBase 客户端的常见要求，特别是在事件流应用程序中，您可能每秒钟会收到数百甚至数千个要在处理器中缓冲的事件。

HappyBase 中的 Batch 类允许您在不编写自定义代码的情况下执行此操作，以维护挂起更新的缓冲区。您可以从表创建批处理对象，并在上下文管理器块中使用它。当块结束时，批处理调用 send（）方法，该方法将所有更新发送到 Thrift 服务器，如代码清单 46 所示：

代码清单 46：使用 HappyBase 批量更新数据

```
>>> with access_logs.batch() as batch:
...     batch.put('elton|jericho|201512', {'t:0110':'200'})
...     batch.put('elton|jericho|201512', {'t:0210':'120', 't:0211':'360'})
...
>>> print access_logs.row('elton|jericho|201512')
{'t:0211': '360', 't:0210': '120', 't:0110': '200'}

```

在 Batch 类上的 put 方法与 Table 类具有相同的签名，因此您可以使用每个对一行进行一次或多次更新]放。

![](img/00015.jpeg) 注意：Thrift API 支持批处理，因此当发送一批更新（在本机 Thrift API 中称为突变）时，这是在对 Thrift 连接的单次调用中完成的。

Thrift 还支持递增计数器列，您可以使用 counter_inc 方法在 HappyBase 中执行，可选择提供一个递增量，如代码清单 47 所示：

代码清单 47：使用 HappyBase 增加计数器

```
>>> counterTable.counter_get('rk1', 'c:1')
1
>>> counterTable.counter_inc('rk1', 'c:1')
2
>>> counterTable.counter_inc('rk1', 'c:1', 100)
102

```

请注意， counter_inc 方法在应用增量后返回单元格值，这与 put（）方法不同，后者没有返回。