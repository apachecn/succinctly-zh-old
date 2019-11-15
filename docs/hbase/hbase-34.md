## 使用 HappyBase 连接到 Thrift

您需要在您的环境中安装 HappyBase。它在 Python Package Index 上公开可用，因此假设您已经拥有 Python 和 Pip（Python 包管理器），您可以使用代码清单 35 中的命令进行安装：

代码清单 35：安装 HappyBase Python 包

```
$ pip install happybase

```

现在，您可以通过使用 import happybase 导入 HappyBase 包来启动 Python 并设置所有依赖项。 HappyBase 旨在以类似 Python 的方式公开 HBase 功能，因此在代码清单 36 中，我们创建了一个连接对象，它将自动连接到本地运行的 HBase Thrift 服务器：

代码清单 36：使用 HappyBase 连接 HBase

```
>>> connection = happybase.Connection('127.0.0.1')

```

Connection 对象是 Thrift 连接的起点。从 Connection 对象，您可以访问表对象，这些对象用于 DDL 和 DML 语句，并获取批处理对象，这些对象用于批量数据更新。