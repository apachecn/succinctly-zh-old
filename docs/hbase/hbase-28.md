## 使用 Java 中的扫描仪

要扫描一系列行，可以使用开始和（可选）停止行键边界创建 Scan 对象，并将其传递给上的 getScanner（）方法]表类。这将在服务器上创建扫描程序并返回可用于迭代行的 ResultScanner 对象。

每次迭代都返回一个 Result 对象，如代码清单 27 所示，我使用辅助方法 printCells（）来编写输出：

代码清单 27：使用 Java 扫描行

```
Table access_logs = connection.getTable(TableName.valueOf("access-logs"));
 Scan scan = new Scan(Bytes.toBytes("elton|jericho|201510"),
                      Bytes.toBytes("elton|jericho|x"));
 ResultScanner scanner = access_logs.getScanner(scan);
 for (Result result : scanner) {
     printCells(result);
 }

 //output - three cells, two whole rows:
 //[elton|jericho|201510] t:2908 = 80
 //[elton|jericho|201511] t:1106 = 120
 //[elton|jericho|201511] t:1107 = 650

```

您可以通过在 Scan 对象上指定属性来调整扫描仪性能：

· setCaching - 指定要在服务器上缓存的行数。较大的缓存值意味着客户端可以以服务器内存为代价更快地迭代扫描器。

· setMaxResultSize - 指定整个扫描仪应返回的最大单元格数。用于验证大型表中数据子集的逻辑。

· setBatch - 指定批次每次迭代返回的最大单元格数。

请注意， Scan 实例在迭代扫描程序时被修改，因此您应该为要执行的每次扫描创建新实例。除了原始的 Connection 对象之外，客户端对象的创建成本低廉，不需要重复使用。

您还可以使用 addFamily（）和 addColumn（）方法限制扫描仪结果中的单元格，其工作方式与 Get 相同]班。