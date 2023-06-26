---
description: st.cache is used to memoize function executions.
slug: /library/api-reference/performance/st.cache
title: st.cache
---

# st.cache

当您使用Streamlit的缓存注解标记一个函数时，它告诉Streamlit在每次调用该函数时应该检查三个方面：

1. 函数的名称
2. 构成函数主体的实际代码
3. 您调用函数时的输入参数

如果这是Streamlit首次看到这三个项目，并且具有相同的值和组合方式，它将运行该函数并将结果存储在本地缓存中。

然后，下次调用该函数时，如果这三个值没有改变，Streamlit会知道可以完全跳过执行该函数。相反，它只需从本地缓存中读取输出，并将其传递给调用者。

主要的限制是Streamlit的缓存功能无法感知到在注释函数的代码块之外发生的变化。

有关Streamlit缓存及其配置参数的更多信息，请参阅相关文档。
有关其限制的详细信息，请参阅[Caching](/library/advanced-features/caching)。

<Autofunction function="streamlit.cache" deprecated={true} deprecatedText="<code>st.cache</code>在1.18.0版本中已弃用。请改用<a href='/library/api-reference/performance/st.cache_data'><code>st.cache_data</code></a>或<a href='/library/api-reference/performance/st.cache_resource'><code>st.cache_resource</code></a>。在<a href='/library/advanced-features/caching'>Caching</a>中了解更多信息。"/>