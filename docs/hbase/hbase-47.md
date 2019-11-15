## .NET 中的扫描仪和过滤器

按行键扫描是从 Stargate 读取数据的最快方法，但如果您需要通过列中的数据（限定符或单元格值）另外限制结果，则可以在创建扫描程序时指定过滤器。

该过滤器还在 Stargate 中运行服务器端，因此它比获取整行和提取客户端中的某些列更快，但它确实使用了额外的服务器资源（如果您在区域服务器上运行 Stargate，则很重要）。

Stargate API 有各种各样的过滤器类型，但有一些最有用的是：

· KeyOnlyFilter - 仅返回扫描仪中的行键

· QualifierFilter - 返回与指定列限定符匹配的单元格

· ColumnPrefixFilter - 返回列名与指定前缀匹配的单元格

在 access-logs 表中，行键中的句点指定使用记录的年份和月份，列限定符包含日期和小时。在代码清单 58 中，我向扫描程序添加了一个列前缀过滤器，它过滤结果，因此只返回列名以以提供的前缀开头的单元格。在这种情况下，只有每个月的第 11 天的单元格才会包含在结果中：

代码清单 58：使用 IStargate 创建过滤扫描仪

```
var options = new ScannerOptions
{
      TableName = "access-logs",
      StartRow = "elton|jericho|201510",
      StopRow = "elton|jericho|x",
      Filter = new ColumnPrefixFilter("11")
};
var scanner = stargate.CreateScanner(options);

```