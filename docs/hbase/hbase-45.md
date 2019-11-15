## 使用.NET 读取数据

Stargate 客户端有两种读取数据的方法。最简单的是 ReadValue（），它获取特定的单元格值，并将表名，行键，列族和限定符传递给它。这在功能上等同于 GET 请求，其 URL 包含表名，行键和列名，它返回编码为字符串的单个单元格值，如代码清单 54 所示：

代码清单 54：使用 IStargate 读取单元格

```
var value = stargate.ReadValue("access-logs", "elton|jericho|201511", "t", "1106");
//value is "120"

```

或者，您可以使用 FindCells（）方法获取 CellSet 对象，该方法返回行和单元格的集合。这就像发出一个完整行的 GET 请求或一行中的列族。 CellSet 是一个可枚举的集合，您可以使用 LINQ 进行查询，如代码清单 55 所示：

代码清单 55：使用 IStargate 查找单元格

```
var cellSet = stargate.FindCells("access-logs", "elton|jericho|201511");
var value = cellSet
             .First(x => x.Identifier.CellDescriptor.Qualifier == "1106").Value;
//value is "120"

```

注意 FindCells（）调用向 Stargate 发出 GET 请求，返回 CellSet 中的所有数据，LINQ 查询在上运行] CellSet 在.NET 客户端的内存中。

ReadValue（）调用将快速返回，因为它正在获取单个数据，并且 FindCells（）调用将很快为 Stargate 提供服务，但客户端可能需要更长时间如果有许多单元格包含大量数据，则接收。

从 Stargate 获取数据的另一种方法是创建行扫描程序，它类似于可用于读取多行的服务器端游标，扫描程序可以选择使用过滤器来限制返回的单元格数。