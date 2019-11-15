## Java 中的扫描仪和过滤器

通过向扫描程序添加过滤器，您可以执行复杂查询。所有选择都发生在服务器端，但您需要记住，虽然行扫描速度很快，但列过滤速度较慢，尤其是对于包含大量列的宽表。

过滤器在 Java API 中是强类型的，继承自抽象 FilterBase 类。有许多不同用途的过滤器 - 包的 Javadoc 中的树视图 org.apache.hadoop.hbase.filter 是一个检查它们的好地方。

ValueFilter 是一个有用的例子;它通过将它们的值与提供的比较运算符和另一个过滤器进行比较来过滤单元格如果将单元格值存储为字符串，则可以过滤对与正则表达式匹配的列值的响应，如代码清单 28 所示：

代码清单 28：使用 Java 扫描和过滤行

```
 scan = new Scan(Bytes.toBytes("elton|jericho|201510"),
                 Bytes.toBytes("elton|jericho|x"));
 scan.setFilter(new ValueFilter(CompareOp.EQUAL,
                 new RegexStringComparator("[5-9][0-9]0")));
 scanner = access_logs.getScanner(scan);
 for (Result result : scanner) {
     printCells(result);
 }

 //output - one cell:
 //[elton|jericho|201511] t:1107 = 650

```

ValueFilter 和 RegexStringComparator 的组合意味着只有在 500 到 990 之间具有三位数值且以零结尾的单元格中才会包含单元格。该过滤器适用于所有系列中的所有列;不需要姓氏或限定符。

使用 Java API，您还可以使用 FilterList 对象组合多个过滤器，并指定包含条件，行是否必须匹配所有过滤器或仅一个。

您可以组合列表中的任何过滤器;代码清单 29 显示了一个列表，它使用正则表达式过滤列限定符名称和单元格值：

代码清单 29：使用 Java 中的多个过滤器进行扫描

```
 FilterList filterList = new  
  FilterList(FilterList.Operator.MUST_PASS_ALL);
 filterList.addFilter(new QualifierFilter(CompareOp.EQUAL,
                       new RegexStringComparator("[0-9]{2}0[7-8]")));
 filterList.addFilter(new ValueFilter(CompareOp.EQUAL,
                       new RegexStringComparator("[0-9]0")));
 scan = new Scan(Bytes.toBytes("elton|jericho|201510"),
                 Bytes.toBytes("elton|jericho|x"));
 scan.setFilter(filterList);
 scanner = access_logs.getScanner(scan);
 for (Result result : scanner) {
     printCells(result);
 }

 //output - two cells:
 //[elton|jericho|201510] t:2908 = 80
 //[elton|jericho|201511] t:1107 = 650

```