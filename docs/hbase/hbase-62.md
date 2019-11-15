## 摘要

在本章中，我们查看了 Region Server，了解数据如何实际存储在 HBase 中，以及 Region Server 如何处理读取和写入数据的请求。

我们看到 HBase 在每个 Region Server 中都有一个读缓存，每个区域都有一个写缓冲区来提高性能，还有一个 Write Ahead Log，以确保服务器出现故障时的数据完整性。

最终，数据存储在 HFile 的磁盘上，其中一个 HFile 逻辑上包含一个表的一个区域中的一个列族的所有数据。但缓冲模式意味着逻辑 HFile 可以在磁盘上拆分为多个存储文件，这可能会损害读取性能。

在下一章中，我们将介绍通过 HMaster Web UI 和 HBase Shell 监控和管理 HBase，包括查找和修复这些性能问题。