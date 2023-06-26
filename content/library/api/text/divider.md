---
title: st.divider
slug: /library/api-reference/text/st.divider
description: st.divider在您的应用中显示一个水平分隔线。
---

<Autofunction function="streamlit.divider" />

当您的应用中有多个元素时，它的效果如下所示:

```python
import streamlit as st

st.write("This is some text.")

st.slider("This is a slider", 0, 100, (25, 75))

st.divider()  # 👈 Draws a horizontal rule

st.write("This text is between the horizontal rules.")

st.divider()  # 👈 Another horizontal rule
```

<Image src="/images/api/st.divider.png" clean />
