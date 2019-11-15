## 用 Java 编写数据

Java API 提供了基本的数据更新，就像 HBase Shell 中的 put 命令一样，但也增加了一些更灵活的功能。

Put 类是 Get 类的写入等价物。您为指定的行键实例化它，然后您可以在通过调用相关表对象上的 put（）方法进行更改之前添加一个或多个列值，如在代码清单 30 中：

代码清单 30：使用 Put in Java 更新单元格

```
Table access_logs = connection.getTable(TableName.valueOf("access-logs"));
 Put log = new Put(Bytes.toBytes("elton|jericho|201511"));
 log.addColumn(Bytes.toBytes("t"),      //family
               Bytes.toBytes("1621"),   //qualifier
               Bytes.toBytes("340"));   //value
 access_logs.put(log);       

 //result - updated cell value:
 //t:1621 = 120

```

您可以将多个单元格值添加到 Put 对象，这将自动在一行上设置多个值，并且 addColumn（）方法的重载允许您指定时间戳细胞。

Put 对象也用于表方法 checkAndPut（），它对单元格进行条件更新。该方法在进行更新之前检查列名称和单元格值。如果提供的值匹配，则自动生成 put;如果没有，行不会改变。

代码清单 31 显示了 checkAndPut（）如何用于向行添加新单元格，但仅限于现有单元格（在该行中或表格的另一行中）具有预期值。在这种情况下，我告诉 HBase 添加一列 t：1622 ，但只有当 t：1621 的值是 34000 时，它才是不，所以不应该更新：

代码清单 31：使用 Java 进行有条件的更新

```
Put newLog = new Put(Bytes.toBytes("elton|jericho|201511"));
 log.addColumn(Bytes.toBytes("t"),
               Bytes.toBytes("1622"),
               Bytes.toBytes("100"));
 access_logs.checkAndPut(Bytes.toBytes("elton|jericho|201511"),
                         Bytes.toBytes("t"), //family
                         Bytes.toBytes("1621"),
                         Bytes.toBytes("34000"),
                         newLog);

 //result - not updated, checked value doesn't match

```

代码清单 32 显示了从 HBase Shell 运行两个 put 方法的结果。细胞 t：1621 的值为 340 ，因此尚未添加新细胞 t：1622 ：

代码清单 32：使用 Java 更新提取单元格

```
hbase(main):002:0> get 'access-logs', 'elton|jericho|201511'
COLUMN                CELL                                                      
 t:1106               timestamp=1447703111745, value=120                       
 t:1107               timestamp=1447703111735, value=650                       
 t:1621               timestamp=1447709413579, value=340                        
3 row(s) in 0.0730 seconds

```

Java API 还允许您对单个批处理中的不同行进行多次更新。相同的 Put 类用于定义更改，并且多个 Put 对象被添加到列表中。该列表与表类上的 batch（）方法一起使用，该方法将更新写入单个服务器调用中，如代码清单 33 所示：

代码清单 33：使用 Java 批量更新单元格

```
 List<Row> batch = new ArrayList<Row>();

 Put put1 = new Put(Bytes.toBytes("elton|jericho|201512"));
 put1.addColumn(Bytes.toBytes("t"),
                Bytes.toBytes("0109"),
                Bytes.toBytes("670"));       
 batch.add(put1);

 Put put2 = new Put(Bytes.toBytes("elton|jericho|201601"));
 put2.addColumn(Bytes.toBytes("t"),
                Bytes.toBytes("0110"),
                Bytes.toBytes("110"));       
 batch.add(put2);

 Put put3 = new Put(Bytes.toBytes("elton|jericho|201602"));
 put3.addColumn(Bytes.toBytes("t"),
                Bytes.toBytes("0206"),
                Bytes.toBytes("500"));       
 batch.add(put3);

 Table access_logs = connection.getTable(TableName.valueOf("access-logs"));
 Object[] results = new Object[batch.size()];   
 access_logs.batch(batch, results);

```

您可以批量包含其他操作，因此可以使用 Put 对象添加删除对象。批次可以包含获取对象以返回一组结果，但不保证批次的顺序 - 所以如果 Get 包含与 Put [相同]的单元格 HTG9]，您可以获得 Put 之前的状态数据。

在代码清单 34 中，执行从 HBase Shell 的 scan 命令中看到的批处理的结果：

代码清单 34：从 Java 批量更新中获取单元格

```
hbase(main):003:0> scan 'access-logs', {STARTROW => 'elton|jericho|201512'}
ROW                   COLUMN+CELL                                              
 elton|jericho|201512 column=t:0109, timestamp=1447710255527, value=670        
 elton|jericho|201601 column=t:0110, timestamp=1447710255527, value=110        
 elton|jericho|201602 column=t:0206, timestamp=1447710255527, value=500        
3 row(s) in 0.0680 seconds

```

![](img/00012.jpeg)提示：这是第一次提到删除 HBase 中的数据。我没有覆盖它，因为我发现你很少这样做，但你可以删除表格中的单个单元格和行。 HBase 使用删除标记来标记已删除的值，而不是立即从磁盘中删除数据，因此删除操作很快。