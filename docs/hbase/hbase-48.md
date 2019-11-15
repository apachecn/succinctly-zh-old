## 从.NET 编写数据

IStargate 接口有两种写入数据的方法。最简单的是 WriteValue（），它相当于一行中特定单元格的 PUT 请求。如果需要，Stargate 会创建行和/或列，并设置值，如代码清单 59 所示：

代码清单 59：使用 IStargate 更新单元格值

```
stargate.WriteValue("100", "access-logs", "elton|jericho|201510", "t", "2908");
//cell value is now "100"

```

更复杂和灵活的方法是 WriteCells（），它采用 CellSet 对象，可以通过单个 API 调用更新多个值。这些值的混合可以包括不同行的更新和插入，但所有行必须在同一个表中。

代码清单 60 显示了对现有单元格值的更新，以及在 WriteCells（）的单个调用中插入新行：

代码清单 60：使用 IStargate 更新单元格值

```
var update = new Cell(new Identifier
{
      Row = "elton|jericho|201510",
      CellDescriptor = new HBaseCellDescriptor
      {
            Column = "t",
            Qualifier = "2908"
      }
}, "120");

var insert = new Cell(new Identifier
{
      Row = "elijah|jericho|201511",
      CellDescriptor = new HBaseCellDescriptor
      {
            Column = "t",
            Qualifier = "1117"
      }
}, "360");

var cells = new CellSet(new Cell[] { update, insert});
cells.Table = "access-logs";

stargate.WriteCells(cells);

```

Stargate API 是无状态的（即使扫描程序在 Region Server 上运行，也不一定是 Stargate 服务器），客户端也是如此，因此本地没有数据缓存，除非您自己将单元保留在内存中。每次调用 Stargate 客户端读取或写入数据都会导致对 Stargate 的 REST 调用。