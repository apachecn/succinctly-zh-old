# 第 6 章与.NET 和 Stargate 连接

## 概述

Stargate 是 HBase REST API 的名称，它使数据可通过 HTTP 进行读写。 Stargate 公开来自与表结构匹配的 URL 的数据（例如， / access-logs / rk1 将获得来自 access-logs 表的密钥 rk1 的行）。

HTTP 动词 GET ， POST 和 DELETE 用于处理数据作为资源，这为 Stargate 提供了一个很好的 RESTful 接口。您可以使用 JSON 中的行和单元格，但 API 的缺点是所有数据都表示为 Base64 字符串，它们是从 HBase 中的原始字节数组编码的。如果你只想浏览像 Postman 或 cURL 这样的 REST 客户端，这会让 API 变得尴尬。

与 Thrift API 一样，Stargate 是一个单独的服务，您可以从 hbase-daemon.sh start rest 开始。默认情况下，它侦听端口 8080（并且已经在 hbase-succinctly Docker 镜像上运行）。

在生产中，您可以在区域服务器上运行 Stargate，但如果要将其用作 HBase 的主要接口，则应考虑使用单独的服务器进行负载平衡。

![](img/00012.jpeg)提示：我在[这篇博客文章](https://blog.sixeyed.com/using-nginx-as-a-load-balancing-proxy-for-stargate)中使用 Nginx 创建一个负载平衡反向代理到前面的 Stargate。

您可以使用任何带有 HTTP 客户端的框架与 Stargate 交谈。在本章中，我们将使用 cURL 查看原始 HTTP 数据，并使用.NET 客户端库在更高级别的抽象中工作。