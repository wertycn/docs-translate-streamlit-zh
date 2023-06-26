---
title: 远程运行时应用程序无法加载
slug: /knowledge-base/deploy/remote-start
---

# 远程运行时应用程序无法加载

以下是用户在远程主机上运行Streamlit应用程序时遇到的一些常见错误。

要了解一种简单的方式来托管Streamlit应用程序并避免下面所有的问题，请查看[Streamlit社区云](https://streamlit.io/cloud)。

### 症状1：应用程序永远无法加载

当您在浏览器中输入应用程序的URL时，如果您只看到一个空白页面，一个"页面未找到"错误，一个"连接拒绝"错误，或者其他类似的错误，请首先检查Streamlit是否实际在远程服务器上运行。在Linux服务器上，您可以通过SSH进入服务器，然后运行以下命令：

```bash
ps -Al | grep streamlit
```

如果您看到Streamlit在运行，最有可能的原因是Streamlit端口没有被暴露出来。解决方法取决于您的具体设置。以下是三个示例修复方法：

- **尝试使用端口80：** 一些主机默认暴露端口80。要将Streamlit设置为使用该端口，请使用`--server.port`选项启动Streamlit：

  ```bash
  streamlit run my_app.py --server.port=80
  ```

- **AWS EC2服务器：** 首先，在[AWS控制台](https://us-west-2.console.aws.amazon.com/ec2/v2/home)中点击您的实例。
  然后向下滚动，点击“安全组” → “入站” → “编辑”。然后，添加一个“自定义TCP”规则，允许“端口范围”为`8501`，来源为`0.0.0.0/0`。

- **其他类型的服务器**: 检查防火墙设置。

如果问题仍未解决，请尝试运行一个简单的HTTP服务器，而不是Streamlit，并查看它是否正常工作。如果是这样，那么您就知道问题出在Streamlit应用程序或配置中（在
如果是这种情况，您应该在我们的[论坛](https://discuss.streamlit.io)上寻求帮助！如果不是这种情况，那肯定与Streamlit无关。

如何启动一个简单的HTTP服务器：

```bash
python -m http.server [port]
```

### 症状2：应用程序一直显示“请稍候...”

如果在尝试在浏览器中加载应用程序时，您看到一个位于页面中央的蓝色框，上面写着“请稍候...”，那么可能的根本原因是以下之一：

- 配置错误的 [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 保护。
- 服务器正在剥离 Websocket 连接的头部，从而导致压缩失效。

为了诊断问题，可以尝试临时禁用 CORS 保护，方法是运行以下命令：
使用 `--server.enableCORS` 标志设置为 `false` 的 Streamlit：

```bash
streamlit run my_app.py --server.enableCORS=false
```

如果这解决了您的问题，**您应该重新启用CORS保护**，然后将`browser.serverPort`和`browser.serverAddress`设置为您的Streamlit应用程序的URL和端口。

如果问题仍然存在，请尝试通过设置Streamlit运行时的`--server.enableWebsocketCompression`标志为`false`来禁用websocket压缩。

```bash
streamlit run my_app.py --server.enableWebsocketCompression=false
```

如果这解决了您的问题，那么您的服务器设置很可能会去掉用于协商WebSocket压缩的`Sec-WebSocket-Extensions` HTTP头。

Streamlit的工作并不需要压缩，但强烈建议开启压缩以提高性能。如果您想重新开启压缩，您需要找到负责去掉`Sec-WebSocket-Extensions` HTTP头的基础设施的部分，并更改该行为。

### 症状3：在多个副本中运行时无法上传文件

如果文件上传小部件返回状态码403的错误，这可能是由于应用程序的[XSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery)保护逻辑配置错误所致。

为了诊断问题，可以尝试通过将Streamlit运行时的`--server.enableXsrfProtection`标志设置为`false`来临时禁用XSRF保护：

```bash
streamlit run my_app.py --server.enableXsrfProtection=false
```

如果这解决了您的问题，**您应该重新启用XSRF保护**，然后通过将`server.cookieSecret`配置选项设置为同一个难以猜测的字符串来配置您的应用程序，在每个副本中都使用相同的密钥。
