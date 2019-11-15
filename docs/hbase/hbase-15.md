## 阅读数据

使用 get 命令读取行上的数据，但与 put 命令不同，您可以使用 get 读取多个单元格值。要读取整行，请使用获取表名和行键。 HBase 将返回行中每个单元格值的最新版本，如代码清单 12 所示：

代码清单 12：读取整行

```
hbase(main):017:0> get 'social-usage', 'a'
COLUMN                          CELL                                                                                   
 i:tw                           timestamp=1446655378543, value=@EltonStoneman                                          
 t:tw                           timestamp=1446655459639, value=900                                                     
 tw:2015110216                  timestamp=1446655423853, value=310                                                      
 tw:2015110316                  timestamp=1446655409785, value=270                                                     
 tw:2015110417                  timestamp=1446655398909, value=320                                                      
5 row(s) in 0.0360 seconds

```

您可以将结果限制为特定的列或族，并使用逗号分隔的列族名称列表或行键后的限定符。在代码清单 13 中，我们返回整个 i 系列，并且只返回 tw 系列中的一列：

代码清单 13：读取特定列

```
hbase(main):022:0> get 'social-usage', 'a', 'tw:2015110316', 'i'
COLUMN                          CELL                                                                                   
 i:tw                           timestamp=1446655378543, value=@EltonStoneman                                          
 tw:2015110316                  timestamp=1446655409785, value=270
2 row(s) in 0.0090 seconds

```

您还可以传递具有许多属性的对象，而不仅仅是字符串，以从行返回更具体的详细信息。我可以通过指定 VERSIONS 属性返回列的多个版本，并返回要返回的数字，如代码清单 14 所示：

代码清单 14：读取多个单元格版本

```
hbase(main):027:0> get 'with-custom-config', 'rk1', {COLUMN =>'cf1:data', VERSIONS => 3}
COLUMN                          CELL                                                                                   
 cf1:data                       timestamp=1446655931606, value=v3                                                      
 cf1:data                       timestamp=1446655929977, value=v2                                                      
 cf1:data                       timestamp=1446655928221, value=v1                                                      
3 row(s) in 0.0120 seconds

```