## 扫描行

HBase 表中的行按顺序物理存储，按行键排序。我们将在第 9 章“区域服务器内部”中查看数据的结构和索引方法，但是现在，我们只需要知道：

· 排序表表示按键快速直接访问。

· 排序表表示按值搜索速度慢。

实际上，您无法在 HBase 中搜索与任意模式匹配的行键。如果您有一个存储系统访问日志的表，其行键启动 {systemID} | {userID} ，则无法搜索某个特定用户的日志，因为用户 ID 位于行键。

使用 HBase，您可以通过扫描表找到匹配的行，为扫描提供开始和结束边界。从逻辑上讲，HBase 的工作方式与游标类似，将表定位到起始行（或起始行的部分匹配）并读取直到结束行。

扫描命令很简单，但对于 HBase 的新用户来说结果可能是意外的。表 2 显示了我的 access-logs 表中的一些示例数据：

| 行键 |
| 杰里科&#124;戴夫&#124; 201510 |
| 杰里科&#124;埃尔顿&#124; 201510 |
| 杰里科&#124;埃尔顿&#124; 201511 |
| 杰里科&#124;弗雷德&#124; 201510 |

表 2：样本行键

我们在这里有四个行密钥，全部用于名为 Jericho 的系统，用于 Dave，Elton 和 Fred，用于 2015 年 10 月和 11 月。表 2 中的行以相同的字典顺序列出，因为它们存储在 HBase 表。

要查找 Jericho 访问的所有行，我可以使用 STARTROW 值扫描表格，如代码清单 15 所示：

代码清单 15：用 STARTROW 扫描

```
hbase(main):006:0> scan 'access-logs', {STARTROW => 'jericho'}
ROW                             COLUMN+CELL                                                                            
 jericho|dave|201510            column=t:3015, timestamp=1446706437576, value=60                                       
 jericho|elton|201510           column=t:3015, timestamp=1446706444028, value=700                                      
 jericho|elton|201511           column=t:0416, timestamp=1446706449473, value=800                                       
 jericho|fred|201510            column=t:0101, timestamp=1446706454401, value=450                                      
4 row(s) in 0.0540 seconds

```

要查找所有 Elton 访问 Jericho 的信息， STARTROW 需要包含用户 ID，我需要添加 ENDROW 值以排除 Elton 之后的任何行。这是扫描变得更有趣的地方。我可以使用 jericho | f 的 ENDROW 值，这样就可以得到 Elton 的日志，如代码清单 16 所示：

代码清单 16：使用 ENDROW 扫描

```
hbase(main):010:0> scan 'access-logs', {STARTROW => 'jericho|elton', ENDROW => 'jericho|f'}
ROW                             COLUMN+CELL                                                                            
 jericho|elton|201510           column=t:3015, timestamp=1446706444028, value=700                                      
 jericho|elton|201511           column=t:0416, timestamp=1446706449473, value=800                                      
2 row(s) in 0.0190 seconds

```

该查询现在有效，但如果我们稍后为名为 Ernie 的用户添加行，则当我运行相同的查询时，它也将返回其日志。因此扫描的 STARTROW 和 ENDROW 需要尽可能具体，而不会失去查询的灵活性。

将在任何时间段内返回所有 Elton 日志的查询可以使用 STARTROW =&gt; “杰里科|埃尔顿|” 和 ENDROW =&gt; 'jericho | elton | x'。了解您的 ASCII 字符代码在这里有所帮助。

管道字符具有比任何字母数字字符更高的值，因此在用户名之后包括管道确保没有其他用户的日志将进入扫描。字符 x 高于任何数字，因此在扫描结束时添加它意味着查询将返回任何年份的行。

关于扫描的最后一点：上边界是'达到'值，而不是'达到并包括'值。如果我想在 2015 年 10 月和 11 月看到所有 Elton 的访问权限，则代码清单 17 中的查询不正确：

代码清单 17：扫描'向上'到 ENDROW

```
hbase(main):014:0> scan 'access-logs', {STARTROW => 'jericho|elton|201510', ENDROW => 'jericho|elton|201511'}
ROW                             COLUMN+CELL                                                                             
 jericho|elton|201510           column=t:3015, timestamp=1446706444028, value=700                                      
1 row(s) in 0.0140 seconds

```

使用 jericho | elton | 201511 的 ENDROW 值表示 HBase 读取该行然后停止。要包含 201511 中的行，我需要一个比那些行更远的结束行值，在这种情况下我可以使用 jericho | elton | 201512 。