---
标题: 如何创建锚点链接？
网址: /knowledge-base/using-streamlit/create-anchor-link
---

# 如何创建锚点链接？

## 概述

您是否想要创建锚点，以便您的应用程序的用户可以通过在URL中指定`#anchor`来直接导航到特定的部分？如果是的话，让我们来看看如何操作。

## 解决方案

锚点会自动添加到标题文本中。

例如，如果您通过[st.header()](/library/api-reference/text/st.header)命令定义一个标题文本，如下所示：

```python
st.header("Section 1")
```

然后，您可以使用以下方式创建一个指向该标题的链接：

```python
st.markdown("[Section 1](#section-1)")
```

## 示例

- 演示应用程序：[https://dataprofessor-streamlit-anchor-app-80kk8w.streamlit.app/](https://dataprofessor-streamlit-anchor-app-80kk8w.streamlit.app/)
- GitHub仓库：[https://github.com/dataprofessor/streamlit/blob/main/anchor_app.py](https://github.com/dataprofessor/streamlit/blob/main/anchor_app.py)
