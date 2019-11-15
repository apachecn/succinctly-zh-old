## 使用带有.NET 的 Stargate NuGet 包

NuGet 是.NET 应用程序的软件包管理器，有几个开源软件包是访问 Stargate 的包装器。 Microsoft 有一个专门针对在 Azure 云上运行的 HBase 集群的软件包，还有来自作者“The Tribe”的第三方软件包，用于与 Stargate 合作。

该软件包可以很好地抽象出 Stargate 的内部结构，并让您直观地处理 HBase 数据。它具有 IoC 感知功能（默认情况下使用 Autofac），因此您可以轻松调整 HTTP 设置并构建数据访问层，您可以模拟测试。

要将该程序包及其依赖项添加到.NET 应用程序，可以使用代码清单 52 中的 NuGet 程序包管理器控制台命令：

代码清单 52：为 Stargate 添加 NuGet 参考

```
Install-Package "HBase.Stargate.Client.Autofac"

```

在本书的 GitHub 存储库中，有一个.NET 控制台应用程序，它使用 The Tribe 的客户端连接到 Stargate，在 hbase-succinctly Docker 容器中运行。