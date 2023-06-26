---
slug: /knowledge-base/deploy/remote-start
title: App is not loading when running remotely
---

# 远程运行时应用程序未加载

以下是用户在自己搭建解决方案以远程托管Streamlit应用程序时遇到的一些常见错误。

要了解一种简单的方式来托管Streamlit应用程序并避免下面的所有问题，请查看[Streamlit Community Cloud](https://streamlit.io/cloud)。

### 症状1：应用程序永远无法加载

当您在浏览器中输入应用程序的URL时，只看到一个**空白页面**，或者只有一些文本而没有应用程序界面时，可能会出现此问题。
如果遇到"Page not found"错误、"Connection refused"错误或类似的错误，请首先检查Streamlit是否实际运行在远程服务器上。在Linux服务器上，您可以通过SSH进入服务器然后运行以下命令:

```bash
ps -Al | grep streamlit
```

如果您看到Streamlit正在运行，最可能的原因是Streamlit端口没有暴露出来。修复方法取决于您的具体设置。以下是三个示例修复方法：

- **尝试使用端口80：** 一些主机默认会暴露端口80。要设置Streamlit使用该端口，请使用`--server.port`选项启动Streamlit：

  ```bash
  streamlit run my_app.py --server.port=80
  ```

- **AWS EC2服务器：** 首先，在[AWS控制台](https://us-west-2.console.aws.amazon.com/ec2/v2/home)中单击您的实例。
  然后向下滚动并点击“安全组”→“入站”→“编辑”。接下来，添加一个自定义的TCP规则，允许端口范围为`8501`，源为`0.0.0.0/0`。

- **其他类型的服务器**: 检查防火墙设置。

如果问题仍然无法解决，请尝试运行一个简单的HTTP服务器而不是Streamlit，并查看是否正常工作。如果正常工作，则说明问题出在Streamlit应用程序或配置中。
如果是这种情况，您应该在我们的[论坛](https://discuss.streamlit.io)上寻求帮助！如果不是，那么它肯定与Streamlit无关。

如何启动一个简单的HTTP服务器：

```bash
python -m http.server [port]
```

### 症状 #2: 应用程序一直显示"请稍候..."

如果在尝试在浏览器中加载应用程序时，您看到页面中央有一个蓝色的框，上面写着"请稍候..."，那么可能是以下原因之一导致的：

- 跨域资源共享（CORS）保护配置错误。
- 服务器从Websocket连接中删除了标头，从而破坏了压缩。

为了诊断问题，可以尝试通过运行以下命令临时禁用CORS保护：
使用`--server.enableCORS`标志将Streamlit设置为`false`：

```bash
streamlit run my_app.py --server.enableCORS=false
```

如果这解决了您的问题，**您应该重新启用CORS保护**，然后将`browser.serverPort`和`browser.serverAddress`设置为您的Streamlit应用程序的URL和端口。

如果问题仍然存在，请尝试通过在运行Streamlit时将`--server.enableWebsocketCompression`标志设置为`false`来禁用websocket压缩。

```bash
streamlit run my_app.py --server.enableWebsocketCompression=false
```

如果这解决了您的问题，那么您的服务器设置可能会剥离用于协商Websocket压缩的`Sec-WebSocket-Extensions` HTTP头。

Streamlit的工作不需要压缩，但强烈建议使用压缩，因为它可以提高性能。如果您想重新启用压缩，您需要找到您基础设施中哪个部分剥离了`Sec-WebSocket-Extensions` HTTP头，并更改该行为。

### 症状3：在多个副本中运行时无法上传文件

如果文件上传小部件返回状态码403的错误，这可能是由于应用程序的[XSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery)保护逻辑配置错误。

为了诊断问题，可以通过将Streamlit运行时的`--server.enableXsrfProtection`标志设置为`false`来临时禁用XSRF保护：

```bash
streamlit run my_app.py --server.enableXsrfProtection=false
```

如果这解决了您的问题，**您应该重新启用XSRF保护**，然后通过将`server.cookieSecret`配置选项设置为同样的难以猜测的字符串，在每个副本中配置您的应用程序使用相同的密钥。