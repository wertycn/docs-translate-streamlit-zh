---
title: 如何从应用标题中移除 "· Streamlit"？
slug: /knowledge-base/using-streamlit/remove-streamlit-app-title
---

# 如何从应用标题中移除 "· Streamlit"？

使用 [`st.set_page_config`](/library/api-reference/utilities/st.set_page_config) 来设置页面标题，将不会在标题后添加 "· Streamlit"。例如：

```python
import streamlit as st

st.set_page_config(
   page_title="Ex-stream-ly Cool App",
   page_icon="🧊",
   layout="wide",
   initial_sidebar_state="expanded",
)
```
