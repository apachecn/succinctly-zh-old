## 使用表格

三个 shell 命令将帮助您开始使用新的 HBase 数据库：

· 列表

· 产生

· 描述

要查看数据库中的所有表，运行 list ，您将看到两种形式的输出：作为纯文本列表，以及作为数组表示。代码清单 7 显示了示例输出：

代码清单 7：清单表

```
hbase(main):001:0> list
TABLE                                                                                                                  
api-logs                                                                                                                
social-usage                                                                                                           
2 row(s) in 0.2550 seconds

=> ["api-logs", "social-usage"]

```

列表命令仅提供表名，而不提供其他详细信息。对于大型数据库，您可以通过为命令提供正则表达式来过滤输出，以匹配表名;例如列表'so。*'将显示以'so'开头的表名。

要创建表，请使用 create 命令，指定表名和列系列名称。 （可选）您可以传递列族的配置设置，这是您可以更改 HBase 存储的单元版本数的方法。代码清单 8 显示了两个 create 命令：

代码清单 8：创建表

```
hbase(main):007:0> create 'with-default-config', 'cf1'
0 row(s) in 1.2380 seconds

=> Hbase::Table - with-default-config
hbase(main):008:0> create 'with-custom-config', {NAME =>'cf1', VERSIONS=>3}
0 row(s) in 1.2320 seconds

=> Hbase::Table - with-custom-config

```

表 with-default-config 有一个列族， cf1 ，未指定配置，因此它将使用 HBase 默认值（包括具有单个单元版本）。表 with-custom-config 也有一个名为 cf1 的列族，但具有指定三个单元版本的自定义配置设置。

![](img/00008.jpeg) 注意：HBase Shell 使用 Ruby 语法，用花括号来定义指定为名称 - 值对的对象和属性，用值'=&gt;'分隔。

要查看表的配置，请使用 describe 命令。输出告诉您表是否已启用客户端访问，并包括所有列系列及其所有设置，如代码清单 9 所示：

代码清单 9：描述表格

```
hbase(main):009:0> describe 'with-custom-config'
Table with-custom-config is ENABLED                                                                                    
with-custom-config                                                                                                      
COLUMN FAMILIES DESCRIPTION                                                                                            
{NAME => 'cf1', DATA_BLOCK_ENCODING => 'NONE', BLOOMFILTER => 'ROW', REPLICATION_SCOPE => '0', VERSIONS => '3', COMPRESS
ION => 'NONE', MIN_VERSIONS => '0', TTL => 'FOREVER', KEEP_DELETED_CELLS => 'FALSE', BLOCKSIZE => '65536', IN_MEMORY =>
'false', BLOCKCACHE => 'true'}                                                                                          
1 row(s) in 0.1240 seconds

```

现在我们有一些具有不同配置的表，我们可以开始使用 HBase Shell 添加和检索数据。