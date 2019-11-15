## 数据类型

HBase 中没有数据类型;所有数据都存储为字节数组 - 单元格值，列限定符和行键。一些 HBase 客户端通过它们的接口传输它们;其他人抽象细节并将所有数据公开为字符串（因此客户端库对 HBase 中的字节数组进行编码和解码）。

代码清单 2 显示了社交用法表中的行如何通过 HBase Shell 访问它，后者将字节数组解码为字符串。代码清单 3 通过 REST API 显示相同的行，它将原始字节数组公开为 Base64 编码的字符串：

代码 2：使用 HBase Shell 读取数据

```
hbase(main):006:0> get 'social-usage', 'A|20151104'
COLUMN                CELL                                                     
 tw:10                timestamp=1447622316218, value=180                       
1 row(s) in 0.0380 seconds

```

代码 3：使用 REST API 读取数据

```
$ curl -H Accept:application/json http://127.0.0.1:8080/social-usage/A%7C20151104
{"Row":[{"key":"QXwyMDE1MTEwNA==","Cell":[{"column":"dHc6MTA=","timestamp":1447622316218,"$":"MTgw"}]}]}

```

请注意，行键必须在 REST 调用中进行 URL 编码，并且 JSON 响应中的所有数据字段都是 Base64 字符串。

如果您专门使用一个客户端，则可以使用平台的本机格式存储数据。代码清单 4 显示了如何将 Java 中的十进制值（1.1）保存到 HBase 中的单元格：

代码清单 4：在 HBase 中存储类型化数据

```
Put newLog = new Put(rowKey);
newLog.addColumn(family, qualifier, Bytes.toBytes(1.1));
access_logs.put(newLog); 

```

如果您使用许多客户端，这可能会导致问题，因为本机类型编码在不同平台上并不相同。如果您使用.NET 客户端读取该单元格，并尝试将字节数组解码为.NET 十进制值，则它可能与您在 Java 中保存的值不同。

![](img/00007.jpeg) 提示：如果您正在使用跨平台解决方案，最好将所有值编码为 UTF-8 字符串，以便任何客户端都可以解码 HBase 中的字节数组并获取同样的结果。