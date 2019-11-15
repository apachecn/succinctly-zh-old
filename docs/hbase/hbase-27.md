## 用 Java 读取数据

使用 Connection 对象，您可以获得对特定表的引用，您可以使用它来读取和写入数据。 Java API 在字节数组级别工作，因此您需要决定如何对数据进行编码（本机，或将所有值转换为字符串），以及对所有数据进行编码和解码。

HBase 客户端包中有辅助类，可以简化它。代码清单 24 显示了如何从连接中获取 Table 对象，并使用 Get 对象获取整行：

代码 24：用 Java 读取一行

```
Table access_logs = connection.getTable(TableName.valueOf("access-logs"));
Get get = new Get(Bytes.toBytes("elton|jericho|201511"));
Result result = access_logs.get(get);

```

请注意，表名是使用 TableName 类创建的，行键使用 Bytes 实用程序类编码为字节。当此代码运行时，整行将在结果对象中，该对象包含完整的字节数组。

结果类有一个 listCells（）方法，它返回 Cell 对象的列表;在这些对象中导航字节数组很麻烦，但另一个辅助类 CellUtil 简化了它。代码清单 25 显示了如何导航 Cell 数组，打印出每个单元格的列名和值：

代码清单 25：使用 Java 读取单元格值

```
for (Cell cell : result.listCells()){
  System.out.println(Bytes.toString(CellUtil.cloneFamily(cell)) + ":" +     
                     Bytes.toString(CellUtil.cloneQualifier(cell)) + " = " +
                     Bytes.toString(CellUtil.cloneValue(cell)));
}

//output -            
//t:1106 = 120
//t:1107 = 650     

```

Get 类可用于从行返回一组受限制的单元格。代码清单 26 显示了使用 addFamily（）方法返回行的一个列族中的单元格，以及 addColumn（）方法来限制对单个单元格的响应。

同样，标识符需要字节数组，因此字节类用于编码字符串值：

代码清单 26：使用 Java 读取特定单元格

```
 get = new Get(Bytes.toBytes("elton|jericho|201511"));
 get.addFamily(Bytes.toBytes("t"));
 result = access_logs.get(get);           
 printCells(result);

 //output - single column family:          
 //t:1106 = 120
 //t:1107 = 650 

 get = new Get(Bytes.toBytes("elton|jericho|201511"));
 get.addColumn(Bytes.toBytes("t"), Bytes.toBytes("1106"));
 result = access_logs.get(get);           
 printCells(result);

 //output - single column:          
 //t:1106 = 120    

```

![](img/00010.jpeg)提示。如果您看到使用 HTable 类的代码示例并使用 Configuration 对象直接实例化它们，则 API 中不推荐使用该代码。我正在使用的更新方式是 ConnectionFactory，Connection 和 Table 类。