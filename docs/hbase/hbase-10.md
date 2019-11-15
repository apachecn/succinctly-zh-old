# 第 2 章 HBase 和 HBase Shell

## HBase 运行模式

您可以在一台计算机上运行 HBase 以用于开发和测试环境。 HBase 支持三种运行模式：独立，伪分布和分布式。分布式模式适用于由 HDFS 支持的完整集群，其中多个服务器运行 HBase 堆栈的不同组件，我们将在第 8 章“HBase 的体系结构”中介绍。

独立模式适用于单个计算机，其中所有组件都在单个 Java 虚拟机中运行，而本地文件系统用于存储而不是 HDFS。伪分布式模式在一台服务器上的不同 JVM 中运行每个 HBase 组件，它可以使用 HDFS 或本地文件系统。

![](img/00007.jpeg)提示：伪分布式模式是本地运行的一个很好的选择 - 您可以在组件之间实现生产方式的分离，但无需运行多台机器。

[这个 HBase 文档](http://hbase.apache.org/)涵盖了在本地安装和运行 HBase，所以我不会在这里复制它，但是在本地运行 HBase 的最简单方法是使用 Docker。 Docker Hub 上有一些 HBase 图像，包括我自己的一个，我已经建立了这个图像。

Docker 是一种应用程序容器技术。容器是一种快速，轻量级的计算单元，可让您在一台计算机上运行多个负载。容器在概念上类似于虚拟机，但在磁盘，CPU 和内存使用方面要轻得多。 Docker 可在 Linux，OS / X 和 Windows 机器上运行。你可以在这里获得安装说明[。 Docker Hub 是预先构建的图像的公共注册表，我的图像可以在](http://www.docker.com/)[这里找到](https://hub.docker.com/r/sixeyed/hbase-succinctly)。