---
description: st.divider displays a horizontal rule in your app.
slug: /library/api-reference/text/st.divider
title: st.divider
---

<Autofunction function="streamlit.divider" />

当应用程序中有多个元素时，它的效果如下所示:

```python
import streamlit as st

st.write("This is some text.")

st.slider("This is a slider", 0, 100, (25, 75))

st.divider()  # 👈 Draws a horizontal rule

st.write("This text is between the horizontal rules.")

st.divider()  # 👈 Another horizontal rule
```

![分割线](/images/api/st.divider.png)