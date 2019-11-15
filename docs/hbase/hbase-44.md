## 正在连接 Stargate

要设置 Stargate 客户端，您需要配置服务器 URL 并构建容器。代码清单 53 显示了如何使用 Autofac 执行此操作：

代码清单 53：配置 Stargate 客户端

```
var builder = new ContainerBuilder();
builder.RegisterModule(new StargateModule(new StargateOptions
{
      ServerUrl = "http://127.0.0.1:8080"
}));

var container = builder.Build();
var stargate = container.Resolve<IStargate>();

```

StargateOptions 对象包含 Stargate（或代理）URL， StargateModule 包含所有其他容器注册。从容器中获取的 IStargate 接口提供对所有 Stargate 客户端操作的访问，具有整洁的抽象和编码为字符串的所有数据项。