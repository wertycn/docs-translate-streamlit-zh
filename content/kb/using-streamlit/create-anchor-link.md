---
slug: /knowledge-base/using-streamlit/create-anchor-link
title: How do I create an anchor link?
---

# 如何创建锚链接？

## 概述

您是否想创建锚链接，以便应用程序的用户可以通过在URL中指定`#anchor`来直接导航到特定部分？如果是，让我们看看如何操作。

## 解决方案

锚链接会自动添加到标题文本中。

例如，如果您通过以下方式使用[st.header()](/library/api-reference/text/st.header)命令定义标题文本：

```python
st.header("Section 1")
```

然后，您可以使用以下方式创建指向此标题的链接：

```python
st.markdown("[Section 1](#section-1)")
```

## 示例

- 演示应用程序: [https://dataprofessor-streamlit-anchor-app-80kk8w.streamlit.app/](https://dataprofessor-streamlit-anchor-app-80kk8w.streamlit.app/)
- GitHub存储库: [https://github.com/dataprofessor/streamlit/blob/main/anchor_app.py](https://github.com/dataprofessor/streamlit/blob/main/anchor_app.py)