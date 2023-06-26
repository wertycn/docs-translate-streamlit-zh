---
slug: /library/api-reference/performance
title: Optimize performance
---

# 优化性能

Streamlit为数据和全局资源提供了强大的[缓存机制](/library/advanced-features/caching)，它们可以使您的应用程序在从网络加载数据、操作大型数据集或执行昂贵的计算时保持高性能。

<TileContainer>

<RefCard href="/library/api-reference/performance/st.cache_data" size="half">

#### 缓存数据

用于缓存返回数据的函数装饰器（例如数据框转换、数据库查询、机器学习推断）。

```python
@st.cache_data
def long_function(param1, param2):
  # Perform expensive computation here or
  # fetch data from the web here
  return data
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.cache_resource" size="half">

#### 缓存资源

用于缓存返回全局资源（例如数据库连接、机器学习模型）的函数装饰器。

```python
@st.cache_resource
def init_model():
  # Return a global resource here
  return pipeline(
    "sentiment-analysis",
    model="distilbert-base-uncased-finetuned-sst-2-english"
  )
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.cache_data.clear" size="half">

#### 清除缓存数据

清除所有内存和磁盘数据缓存。

```python
@st.cache_data
def long_function(param1, param2):
  # Perform expensive computation here or
  # fetch data from the web here
  return data

if st.checkbox("Clear All"):
  # Clear values from *all* cache_data functions
  st.cache_data.clear()
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.cache_resource.clear" size="half">

#### 清除缓存资源

清除所有 `st.cache_resource` 的缓存。

```python
@st.cache_resource
def init_model():
  # Return a global resource here
  return pipeline(
    "sentiment-analysis",
    model="distilbert-base-uncased-finetuned-sst-2-english"
  )

if st.checkbox("Clear All"):
  # Clear values from *all* cache_resource functions
  st.cache_data.clear()
```

</RefCard>

</TileContainer>

<Important>

所有下面的命令在1.18.0版本中已经被弃用。请使用上面的新命令代替。在[Caching](/library/advanced-features/caching)中了解更多。

</Important>

## 弃用的命令

<TileContainer>

<RefCard href="/library/api-reference/performance/st.cache" deprecated={true}>

> 这个命令在1.18.0版本中已经被弃用。请使用`st.cache_data`或`st.cache_resource`代替。

#### 缓存

用于缓存函数执行结果的函数装饰器。

```python
@st.cache(ttl=3600)
def run_long_computation(arg1, arg2):
  # Do stuff here
  return computation_output
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.experimental_memo" deprecated={true}>

> 这个命令在1.18.0版本中已经被弃用，请使用`st.cache_data`代替。

#### Memo

实验性的函数装饰器，用于缓存函数的执行结果。

```python
@st.experimental_memo
def fetch_and_clean_data(url):
  # Fetch data from URL here, and then clean it up.
  return data
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.experimental_singleton" deprecated={true}>

> 此命令在1.18.0版本中已废弃。请改用`st.cache_resource`。

#### 单例模式

实验性的函数装饰器，用于存储单例对象。

```python
@st.experimental_singleton
def get_database_session(url):
  # Create a database session object that points to the URL.
  return session
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.experimental_memo.clear" deprecated={true}>

> 此命令已在版本1.18.0中废弃。请使用`st.cache_data.clear`代替。

#### 清除备忘录

清除所有内存和磁盘备忘录缓存。

```python
@st.experimental_memo
def fetch_and_clean_data(url):
  # Fetch data from URL here, and then clean it up.
  return data

if st.checkbox("Clear All"):
  # Clear values from *all* memoized functions
  st.experimental_memo.clear()
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.experimental_singleton.clear"  deprecated={true}>

> 此命令在版本1.18.0中已弃用。请改用`st.cache_resource.clear`。

#### 清除单例缓存

清除所有单例缓存。

```python
@st.experimental_singleton
def get_database_session(url):
  # Create a database session object that points to the URL.
  return session

if st.button("Clear All"):
  # Clears all singleton caches:
  st.experimental_singleton.clear()
```

</RefCard>
</TileContainer>