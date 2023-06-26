---
slug: /library/advanced-features/static-file-serving
title: Static file serving
---

# 静态文件服务

Streamlit应用程序可以托管和提供小型静态媒体文件，以支持媒体嵌入的使用场景，这些场景无法使用普通的[媒体元素](https://docs.streamlit.io/library/api-reference/media)。

要启用此功能，在配置文件的`[server]`下设置`enableStaticServing = true`，
或者在环境变量中设置`STREAMLIT_SERVER_ENABLE_STATIC_SERVING=true`。

位于运行的应用程序文件相对路径`./static/`的文件夹中的媒体文件将在路径中提供服务。
`app/static/[文件名]`，例如 `http://localhost:8501/app/static/cat.png`。

## 使用细节

- 具有以下扩展名的文件将正常提供服务：`".jpg", ".jpeg", ".png", ".gif"`。任何其他文件将以`Content-Type:text/plain`的头部发送，这将导致浏览器以纯文本形式呈现。这是出于安全考虑 - 需要呈现的其他文件类型应该在应用程序外部进行托管。
- Streamlit还为从静态目录渲染的所有文件设置`X-Content-Type-Options:nosniff`。
- 对于在Streamlit Community Cloud上运行的应用程序：
  - 可在Github repo中找到的文件将始终被提供。在应用程序运行期间生成的任何文件（如基于用户交互的文件上传等）不能保证在用户会话之间持久存在。
  - 存储和提供许多文件或大文件的应用程序可能会遇到资源限制并被关闭。

## 示例用法

- 将图像`cat.png`放入文件夹`./static/`
- 在`.streamlit/config.toml`文件中的`[server]`下添加`enableStaticServing = true`
- `./static/`文件夹中的任何媒体将以相对URL的形式提供，例如`app/static/cat.png`

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