## 在.NET 中使用扫描仪

使用 Stargate 扫描行有两个部分。首先，您创建在服务器上运行的扫描程序。 Stargate 为您提供了扫描仪的参考，您可以使用它来获取行。在.NET 客户端中，使用 ScannerOptions 对象指定扫描的开始行和结束行。

代码 56 显示了如何创建一个扫描程序，以便从 2015 年 10 月起为一个系统检索一个用户的所有访问日志：

代码清单 56：使用 IStargate 创建扫描仪

```
var options = new ScannerOptions
{
      TableName = "access-logs",
      StartRow = "elton|jericho|201510",
      StopRow = "elton|jericho|x",
};
var scanner = stargate.CreateScanner(options);

```

当 Stargate 返回时，扫描程序在服务器上运行，您可以使用 MoveNext（）方法从 Stargate 获取下一组单元格，从而从开始到结束行循环遍历行，如代码 57 中：

代码清单 57：通过 IStargate 扫描器进行迭代

```
var totalUsage = 0;
while (scanner.MoveNext())
{
      var cells = scanner.Current;
      foreach (var cell in cells)
      {
            totalUsage += int.Parse(cell.Value);
      }
}
//totalUsage is 850

```

![](img/00015.jpeg) 注意：迭代扫描器时返回的单元格可能来自多行，因此不要假设 MoveNext（或其他客户端中的等价物）移动到下一行 - 它继续移动到下一组单元格，可以来自许多行。