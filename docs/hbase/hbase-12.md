## 使用 HBase Shell

HBase 附带一个命令行界面，即 HBase Shell。 Shell 无法在远程计算机上使用，因此您需要从本地计算机（对于 HBase 独立和伪分布式模式）运行它，或者登录到主服务器（对于分布式模式）。

从 HBase bin 目录中，运行 hbase shell 以启动 Shell。如果您通过我的 Docker 镜像运行 HBase，请通过运行代码清单 6 中的交互式命令进行连接：

代码清单 6：在 Docker 中运行 HBase Shell

```
docker exec -it hbase hbase shell

```

HBase Shell 基于 JRuby，您可以使用它来执行脚本文件以及交互式命令。 Shell 中提供了大量命令。它是开始使用 HBase 的理想之地，您还可以在生产中使用它来管理集群。

我们将在本章中介绍定义表和读写数据的主要命令。