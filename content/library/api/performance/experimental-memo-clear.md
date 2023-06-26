---
description: st.experimental_memo.clear clears all in-memory and on-disk memo caches.
slug: /library/api-reference/performance/st.experimental_memo.clear
title: st.experimental_memo.clear
---

<重要>

这是一个实验性功能。实验性功能及其API可能随时更改或删除。要了解更多信息，请点击[这里](/library/advanced-features/prerelease#experimental-features)。

</重要>

<Autofunction function="streamlit.experimental_memo.clear" deprecated={true} deprecatedText="<code>st.experimental_memo.clear</code>在1.18.0版本中已被弃用。请使用<a href='/library/api-reference/performance/st.cache_data.clear'><code>st.cache_data.clear</code></a>代替。在<a href='/library/advanced-features/caching'>Caching</a>中了解更多信息。"/>

#### 示例

在下面的示例中，按下"清除全部"按钮将会清除所有使用`@st.experimental_memo`装饰的函数的记忆值。

```python
import streamlit as st

@st.experimental_memo
def square(x):
    return x**2

@st.experimental_memo
def cube(x):
    return x**3

if st.button("Clear All"):
    # Clear values from *all* memoized functions:
    # i.e. clear values from both square and cube
    st.experimental_memo.clear()
```