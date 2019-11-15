# 第 5 章用 Python 和 Thrift 连接

## 概述

HBase 非常适合跨平台解决方案，Thrift API 是 Java API 的替代品。 Thrift 是 Apache 的通用 API 接口，支持从不同语言到 Java 服务器的客户端连接。我们将在本章中使用 Python，但您可以使用 Thrift 绑定的任何语言（包括 Go，C＃，Haskell 和 Node）。

Thrift API 是一个外部接口，因此需要额外的 JVM 才能运行。您可以使用 HBase 守护程序脚本 hbase-daemon.sh start thrift 启动它。它可以单独托管到 HBase 集群的其余部分，也可以在 Region Servers 上运行。 Thrift 没有本机负载均衡器，但传输是 TCP，因此您可以使用外部负载均衡器（如 HAProxy）。

默认情况下，Thrift 服务器侦听端口 9090，它已经在 hbase-succinctly Docker 镜像上运行。

Thrift 比 REST 更轻量级，因此它可以提供更好的性能，但它不是那么用户友好。要使用 Thrift API，在大多数情况下，您需要从源代码构建 Thrift，从描述接口的公共 .thrift 文件生成对 API 的绑定，然后导入 Thrift 传输和绑定你的客户端应用。

![](img/00013.jpeg) 注意：Thrift API 记录在.thrift 文件中。该文件不附带 HBase 二进制文件，因此您需要从源中获取正确的版本。对于 1.1.2 版，该文件位于 GitHub [](https://github.com/apache/hbase/blob/cc2b70cf03e3378800661ec5cab11eb43fafe0fc/hbase-thrift/src/main/resources/org/apache/hadoop/hbase/thrift2/hbase.thrift) 。