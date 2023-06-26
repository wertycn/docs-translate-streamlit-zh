---
title: 静态文件服务
slug: /library/advanced-features/static-file-serving
---

# 静态文件服务

Streamlit应用程序可以托管和提供小型静态媒体文件，以支持媒体嵌入用例，这些用例无法使用正常的[媒体元素](https://docs.streamlit.io/library/api-reference/media)进行操作。

要启用此功能，请在配置文件的`[server]`下设置`enableStaticServing = true`，或者在环境变量中设置`STREAMLIT_SERVER_ENABLE_STATIC_SERVING=true`。

存储在与运行应用程序文件相对路径为`./static/`的文件夹中的媒体文件将在路径`app/static/[filename]`下提供服务，例如`http://localhost:8501/app/static/cat.png`。

## 用法详细信息

- 具有以下扩展名的文件将正常提供服务：`".jpg", ".jpeg", ".png", ".gif"`。其他任何文件将使用`Content-Type:text/plain`头发送，这将导致浏览器以纯文本形式呈现。
  这是为了安全起见而包含的内容 - 其他需要渲染的文件类型应该在应用程序外部进行托管。
- Streamlit还为从静态目录渲染的所有文件设置`X-Content-Type-Options:nosniff`。
- 对于在Streamlit Community Cloud上运行的应用程序：
  - Github仓库中可用的文件将始终被提供。在应用程序运行时生成的任何文件（例如基于用户交互的文件上传等）不能保证跨用户会话持久存在。
  - 存储和提供许多文件或大文件的应用程序可能会遇到资源限制并被关闭。

## 示例用法

- 在文件夹`./static/`中放置一张名为`cat.png`的图片
- 在`.streamlit/config.toml`文件中的`[server]`下添加`enableStaticServing = true`
- `./static/`文件夹中的任何媒体文件都可以通过相对URL `app/static/cat.png` 进行访问。

```toml
# .streamlit/config.toml

[server]
enableStaticServing = true
```

```python
# app.py
import streamlit as st

with st.echo():
    st.title("CAT")

    st.markdown("[![Click me](app/static/cat.png)](https://streamlit.io)")

```

附加资源:

- <https://docs.streamlit.io/library/advanced-features/configuration>
- <https://static-file-serving.streamlit.app/>

<Cloud src="https://static-file-serving.streamlit.app/?embedded=true" height="1000" />
