---
title: st.cache
slug: /library/api-reference/performance/st.cache
description: st.cache用于缓存函数执行结果。
---

# st.cache

当您使用Streamlit的缓存注解标记一个函数时，它告诉Streamlit在每次调用函数时应该检查三件事情：

1. 函数的名称
2. 函数体的实际代码
3. 调用函数时的输入参数

如果这是Streamlit第一次看到这三个项目，并且具有这些确切的值和确切的组合，它将运行该函数并将结果存储在本地缓存中。

然后，下次调用该函数时，如果这三个值没有发生变化，Streamlit就知道可以完全跳过执行该函数。相反，它只是从本地缓存中读取输出并将其传递给调用者。

主要限制在于Streamlit的缓存功能不知道...
在注释函数的主体外部发生的更改。

有关Streamlit缓存、其配置参数和限制的更多信息，请参阅[Caching](/library/advanced-features/caching)。

<Autofunction function="streamlit.cache" deprecated={true} deprecatedText="<code>st.cache</code>在1.18.0版本中已被弃用。请使用<a href='/library/api-reference/performance/st.cache_data'><code>st.cache_data</code></a>或<a href='/library/api-reference/performance/st.cache_resource'><code>st.cache_resource</code></a>替代。在<a href='/library/advanced-features/caching'>Caching</a>中了解更多信息。"/>
