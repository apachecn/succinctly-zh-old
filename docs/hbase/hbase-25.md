# 第 4 章使用 Java API 连接

## 概述

客户端连接到 HBase 的本机 API 是 Java API。功能可以分为两部分 - 连接到主服务器的元数据和管理功能，以及连接到区域服务器的数据访问功能（我们将在第 8 章“更深入地介绍这些服务器” HBase 的”）。

您不需要做任何事情来支持服务器上的 Java API，除了确保端口是打开的（默认情况下，Zookeeper 为 2181，Master 为 60000，区域服务器为 60020）。

HBase 客户端软件包位于 Maven 资源库中，其中包含所有已发布版本的 HBase 的 JAR 文件。在撰写本文时，最新版本是 1.1.2（这是我在课程的 Docker 容器中使用的版本），但 0.9x 版本很常见，仍然可以在 Maven 中使用。

![](img/00011.jpeg) 注意：我不会在这里介绍如何使用 Maven 或 Java IDE，但本书的源代码包含一个 [NetBeans Java 项目](https://github.com/sixeyed/hbase-succinctly/tree/master/java)，其示例代码为使用 Maven。