## 使用 Java 客户端连接到 HBase

HBase Java 客户端在 Maven Central 存储库中可用，并且版本化，因此客户端版本号与服务器版本匹配。对于当前版本的 HBase（以及在 hbase-succinctly Docker 容器中运行的版本），我们需要依赖于 org.apache.hbase.hbase-client 的版本 1.1.2 包，如代码清单 21 所示：

代码清单 21：Maven HBase 客户端依赖关系

```
<dependency>
   <groupId>org.apache.hbase</groupId>
   <artifactId>hbase-client</artifactId>
   <version>1.1.2</version>
 </dependency>

```

使用 Java API，您可以从 Configuration 对象开始，该对象包含服务器的连接详细信息，并在为表或管理创建客户端对象时使用它。创建配置对象时，默认情况下，它将在包含配置设置的正在运行的应用程序的资源中查找 hbase-site.xml 文件。

hbase-site.xml 配置文件也存在于服务器上，您可以使用相同的内容进行客户端连接 - 它指定了服务器端口和 Zookeeper 仲裁地址等关键详细信息。代码清单 22 显示了站点文件中的一些示例属性：

代码 22：hbase-site.xml 配置文件

```
<configuration>
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
    <property>
        <name>hbase.master.port</name>
        <value>60000</value>
    </property>
    ...
</configuration>

```

Java 客户端只需知道 Zookeeper 仲裁地址;它从 Zookeeper 获取主服务器和区域服务器地址。

![](img/00011.jpeg) 注意：Zookeeper 将地址存储为主机名而不是 IP，因此您需要确保运行 Java 客户端的计算机可以访问区域服务器的主机名。如果您使用代码清单 6 中的 Docker 运行命令，那么主机名将是 hbase，您应该在主机文件中添加一行，将 hbase 与 127.0.0.1 相关联

您使用 ConnectionFactory 类连接到 HBase 以创建 Connection 对象，该对象使用本地 hbase-site.xml 文件中的配置，如代码中所示清单 23：

代码清单 23：获取与 HBase 的连接

```
Configuration config = HBaseConfiguration.create();
Connection connection = ConnectionFactory.createConnection(config);

```

您可以在代码中设置配置对象的属性，但使用服务器的 XML 配置文件更易于管理。

连接对象很昂贵，应该重复使用。它们用于为 DML 和 DDL 操作创建表和 Admin 对象。当您的数据访问完成时，应关闭 Connection 对象，通常在最终块内调用 close（）。