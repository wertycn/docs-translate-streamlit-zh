---
description: st.experimental_singleton.clear clears all singleton caches.
slug: /library/api-reference/performance/st.experimental_singleton.clear
title: st.experimental_singleton.clear
---

<重要>

这是一个实验性功能。实验性功能及其API可能随时更改或移除。要了解更多信息，请点击[这里](/library/advanced-features/prerelease#experimental-features)。

</重要>

<Autofunction function="streamlit.experimental_singleton.clear" deprecated={true} deprecatedText="<code>st.experimental_singleton.clear</code>在1.18.0版本中已被弃用。请使用<a href='/library/api-reference/performance/st.cache_resource.clear'><code>st.cache_resource.clear</code></a>代替。了解更多信息，请参阅<a href='/library/advanced-features/caching'>缓存</a>。"/>

#### 示例

在下面的示例中，按下"清除全部"按钮将清除所有单例缓存。即清除所有使用`@st.experimental_singleton`装饰的函数中缓存的单例对象。

```python
import streamlit as st
from transformers import BertModel

@st.experimental_singleton
 def get_database_session(url):
     # Create a database session object that points to the URL.
     return session

@st.experimental_singleton
def get_model(model_type):
    # Create a model of the specified type.
    return BertModel.from_pretrained(model_type)

if st.button("Clear All"):
    # Clears all singleton caches:
    st.experimental_singleton.clear()
```