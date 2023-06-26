---
title: HTTPS支持
slug: /library/advanced-features/https-support
---

# HTTPS支持

许多应用程序需要通过SSL / [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security)协议或`https://`访问。

我们建议在反向代理或负载均衡器中进行SSL终止，用于自托管和生产环境，而不是直接在应用程序中进行。[Streamlit社区云](/streamlit-community-cloud)采用这种方法，每个主要的云和应用托管平台都应该允许您进行配置并提供广泛的文档。您可以在我们的[部署教程](/knowledge-base/tutorials/deploy)中找到一些这样的平台。

在您的Streamlit应用程序中终止SSL连接，您必须配置`server.sslCertFile`和`server.sslKeyFile`。了解如何在[配置](/library/advanced-features/configuration)中设置配置选项。

## 使用详情

- 配置值应该是证书文件和密钥文件的本地文件路径。这些文件必须在应用程序启动时可用。
- 必须同时指定`server.sslCertFile`和`server.sslKeyFile`。如果只指定其中一个，您的应用程序将以错误退出。
- 此功能在Community Cloud中不起作用。Community Cloud已经使用TLS为您的应用提供服务。

<警告>

在生产环境中，我们建议通过负载均衡器或反向代理执行SSL终止，而不是使用此选项。在Streamlit中使用此选项尚未经过广泛的安全审计或性能测试。

</警告>

## 示例用法

```toml
# .streamlit/config.toml

[server]
sslCertFile = '/path/to/certchain.pem'
sslKeyFile = '/path/to/private.key'
```
