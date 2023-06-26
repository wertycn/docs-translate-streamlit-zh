---
description: st.cache_data.clear clears all in-memory and on-disk data caches.
slug: /library/api-reference/performance/st.cache_data.clear
title: st.cache_data.clear
---

<Autofunction function="streamlit.cache_data.clear" />

#### 示例

在下面的示例中，按下"清除全部"按钮将清除所有使用`@st.cache_data`修饰的函数的备忘值。

```python
import streamlit as st

@st.cache_data
def square(x):
    return x**2

@st.cache_data
def cube(x):
    return x**3

if st.button("Clear All"):
    # Clear values from *all* all in-memory and on-disk data caches:
    # i.e. clear values from both square and cube
    st.cache_data.clear()
```