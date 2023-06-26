---
description: st.cache_resource.clear clears all cache_resource caches.
slug: /library/api-reference/performance/st.cache_resource.clear
title: st.cache_resource.clear
---

<Autofunction function="streamlit.cache_resource.clear" />

#### 示例

在下面的示例中，点击"Clear All"按钮将清除所有的`cache_resource`缓存。即清除所有使用`@st.cache_resource`装饰的函数缓存的全局资源。

```python
import streamlit as st
from transformers import BertModel

@st.cache_resource
 def get_database_session(url):
     # Create a database session object that points to the URL.
     return session

@st.cache_resource
def get_model(model_type):
    # Create a model of the specified type.
    return BertModel.from_pretrained(model_type)

if st.button("Clear All"):
    # Clears all st.cache_resource caches:
    st.cache_resource.clear()
```