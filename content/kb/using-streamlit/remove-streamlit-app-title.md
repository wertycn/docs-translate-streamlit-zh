---
slug: /knowledge-base/using-streamlit/remove-streamlit-app-title
title: How to remove "· Streamlit" from the app title?
---

# 如何从应用程序标题中删除“· Streamlit”？

使用[`st.set_page_config`](/library/api-reference/utilities/st.set_page_config)来分配页面标题将不会在该标题后面追加“· Streamlit”。例如：

```python
import streamlit as st

st.set_page_config(
   page_title="Ex-stream-ly Cool App",
   page_icon="🧊",
   layout="wide",
   initial_sidebar_state="expanded",
)
```