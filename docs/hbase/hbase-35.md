## 用 Python 读取数据

HappyBase 使 HBase 交互变得非常简单。在连接上使用 table（）方法获取表对象，您可以使用表格中的行方法读取单个单元格，列族或者整排。结果以字典形式返回;代码清单 37 显示了为行，列族和单元格返回的值：

代码清单 37：使用 HappyBase 读取一行

```
>>> table = connection.table('access-logs')
>>> print table.row('elton|jericho|201511')
{'t:1106': '120', 't:1107': '650'}
>>> print table.row('elton|jericho|201511', ['t'])
{'t:1106': '120', 't:1107': '650'}
>>> print table.row('elton|jericho|201511', ['t:1106'])
{'t:1106': '120'}

```

您还可以通过提供 rows（）方法的键列表来读取多行，该方法返回包含每行元组的列表。元组包含行键和列值字典，如代码清单 38 所示，其中返回两行：

代码清单 38：使用 HappyBase 读取多行

```
>>> print table.rows(['elton|jericho|201511', 'elton|jericho|201510'])

[('elton|jericho|201511', {'t:1106': '120', 't:1107': '650'}), ('elton|jericho|201510', {'t:2908': '80'})]

```

键列表是一组明确的键，而不是一个范围的起点和终点（因为你需要一个扫描仪，我们将在下一节中使用它）。如果您请求的密钥不存在，则不会在响应中返回。如果您请求的密钥都不存在，那么您将返回一个空列表。

rows（）方法还允许按列族或列进行过滤;如果您请求行键列表中的行不存在的列，则不会在响应中返回该行。在代码清单 39 中，请求来自两行的 t：1106 列，但只有一行具有该列，因此不返回另一行：

代码清单 39：使用 HappyBase 过滤多行的列

```
>>> print table.rows(['elton|jericho|201511', 'elton|jericho|201510'], ['t:1106'])
[('elton|jericho|201511', {'t:1106': '120'})]

```

row（）和 rows（）方法可以包含一个选项，用于返回响应中每个单元格的时间戳，但如果您在列族中有一个包含多个版本的表，这些方法只返回最新版本。

要从列中读取多个版本，HappyBase 具有 cells（）方法，该方法获取行键和列名以及要返回的版本数（以及可选的数据时间戳） ，如代码清单 40 所示：

代码清单 40：使用 HappyBase 读取多个单元格版本

```
>>> versionedTable = connection.table('with-custom-config')
>>> print versionedTable.cells('rk1', 'cf1:data', 3)
['v2', 'v1', 'v0']

>>> print versionedTable.cells('rk1', 'cf1:data', 3, include_timestamp=True)
[('v2', 1447399969699), ('v1', 1447399962115), ('v0', 1447399948404)]

```

cells（）方法按时间戳的降序返回单元版本。

对于具有计数器列的行，数据将在 row（）和 cells（）方法中返回，但是采用不友好的十六进制格式。 HappyBase 还包括 counter_get 方法，用于将计数器列的当前值读取为长整数。

代码清单 41 显示了读取计数器列的不同结果：

代码清单 41：使用 HappyBase 读取计数器列

```
>>> counterTable = connection.table('counters')
>>> print counterTable.row('rk1')
{'c:1': '\x00\x00\x00\x00\x00\x00\x00\x01'}

>>> print counterTable.counter_get('rk1', 'c:1')
1

```