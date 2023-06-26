---
slug: /library/advanced-features/experimental-cache-primitives
title: Experimental cache primitives
---

<Deprecation>

本页面描述的实验性缓存原语已在1.18.0版本中弃用。请改用[`st.cache_data`](/library/api-reference/performance/st.cache_data)或[`st.cache_resource`](/library/api-reference/performance/st.cache_resource)。有关更多信息，请参阅[Caching](/library/advanced-features/caching)。

</Deprecation>

# 实验性缓存原语

## 概述

Streamlit独特的执行模型是使其使用起来如此愉快的一部分：您的代码会按照顶部到底部的顺序执行，就像一个简单的脚本一样，适用于每个交互。无需考虑模型、视图、控制器或任何其他相关内容。

每当您的代码重新执行时，一个名为[`@st.cache`](/library/api-reference/performance/st.cache)的装饰器将提供一个缓存机制，它是一个强大的用于记忆化和状态存储能力的原语，使您的应用程序在从网络加载数据、操作大型数据集或执行昂贵计算时保持高性能。

然而，我们发现[`@st.cache`](/library/advanced-features/caching)使用起来很困难，而且不够快速。您可能会遇到像`InternalHashError`或`UnhashableTypeError`这样的晦涩错误。或者您需要理解诸如[`hash_funcs`](/library/advanced-features/caching#the-hash_funcs-parameter)和[`allow_output_mutation`](/library/advanced-features/caching#example-1-pass-a-database-connection-around)这样的概念。

我们的解决方案包括两个新的原语：[**`st.experimental_memo`**](/library/api-reference/performance/st.experimental_memo) 和 [**`st.experimental_singleton`**](/library/api-reference/performance/st.experimental_singleton)。它们在概念上更简单，速度也更快。在我们内部对缓存大型数据帧的一些测试中，`@st.experimental_memo` 的性能比 `@st.cache` 快了一个数量级。这意味着速度快了10倍以上！🚀

让我们来看看这两个实验性API的用例，以及它们相比`@st.cache`的重大改进之处。

## 问题

`@st.cache` 主要用于以下用例：

1. 存储给定不同类型输入的计算结果。在计算机科学文献中，这被称为[**记忆化**](https://en.wikipedia.org/wiki/Memoization)。
2. 在Streamlit服务器的生命周期内，仅初始化一个对象，并在每次重新运行时重复使用该实例。这被称为**单例模式**。
3. 存储全局状态，以便在多个Streamlit会话中共享和修改（由于Streamlit是多线程的，因此需要特别注意线程安全性）。

由于`@st.cache`试图在一个统一的API下涵盖过多的用例，它既慢又复杂。

## 解决方案

虽然`@st.cache`试图同时解决两个非常不同的问题（缓存数据和共享全局单例对象），但这些新的原语通过将问题划分到两个不同的API中来简化事情。因此，它们更快且更简单。

### `@st.experimental_memo`

使用[`@st.experimental_memo`](/library/api-reference/performance/st.experimental_memo)来存储昂贵的计算，可以以传统意义上的"缓存"或"备忘录"的方式进行。它几乎具有与现有的`@st.cache`完全相同的API，因此通常可以将一个替换为另一个。

```python
import streamlit as st

@st.experimental_memo
def factorial(n):
	if n < 1:
		return 1
	return n * factorial(n - 1)

f10 = factorial(10)
f9 = factorial(9)  # Returns instantly!
```

#### 属性

- 与`@st.cache`不同，这个方法返回的是缓存项的值，而不是引用。这意味着您不再需要担心意外更改缓存中存储的项。在幕后，这是通过使用Python的`pickle()`函数来对缓存的值进行序列化/反序列化来实现的。
- 尽管这个使用了一个自定义的哈希解决方案来生成缓存键（类似于`@st.cache`），但它**_不_**使用`hash_funcs`作为无法哈希的参数的逃生通道。相反，我们允许您通过在参数前加下划线来忽略无法哈希的参数（例如数据库连接）。

例如：

```python
import streamlit as st
import pandas as pd
from sqlalchemy.orm import sessionmaker

@st.experimental_memo
def get_page(_sessionmaker, page_size, page):
	"""Retrieve rows from the RNA database, and cache them.

	Parameters
	----------
	_sessionmaker : a SQLAlchemy session factory. Because this arg name is
	                prefixed with "_", it won't be hashed.
	page_size : the number of rows in a page of result
	page : the page number to retrieve

	Returns
	-------
	pandas.DataFrame
	A DataFrame containing the retrieved rows. Mutating it won't affect
	the cache.
	"""
	with _sessionmaker() as session:
		query = (
			session
				.query(RNA.id, RNA.seq_short, RNA.seq_long, RNA.len, RNA.upi)
				.order_by(RNA.id)
				.offset(page_size * page)
				.limit(page_size)
		)

		return pd.read_sql(query.statement, query.session.bind)
```

### `@st.experimental_singleton`

[`@st.experimental_singleton`](/library/api-reference/performance/st.experimental_singleton) 是一个在 Streamlit 应用程序的所有会话中共享的键值存储空间。它非常适合在会话之间存储重量级的单例对象（比如 TensorFlow/Torch/Keras 会话和/或数据库连接）。

示例用法：

```python
import streamlit as st
from sqlalchemy.orm import sessionmaker

@st.experimental_singleton
def get_db_sessionmaker():
	# This is for illustration purposes only
	DB_URL = "your-db-url"
	engine = create_engine(DB_URL)
	return sessionmaker(engine)

dbsm = get_db_sessionmaker()
```

#### 与 `@st.cache` 的比较：

- 与 `@st.cache` 类似，**这也是通过引用返回项目。**
- 您可以返回任何类型的对象，包括不可序列化的对象。
- 与 `@st.cache` 不同的是，这个装饰器没有额外的逻辑来检查您是否意外地改变了缓存的对象。那个逻辑很慢，并产生令人困惑的错误信息。因此，我们希望通过将这个装饰器称为 "singleton"，来引导您采取正确的行为。
- 这不符合计算图的规则。
- 您不必担心`hash_funcs`！只需在参数前面加上下划线以忽略它们。

<警告>

单例对象可以被连接到您的应用程序的每个用户并发使用，而且_您有责任确保`@st.singleton`对象是线程安全的_。（大多数您想要放入`@st.singleton`注释中的对象可能已经是安全的，但您应该进行验证。）

</警告>

### 选择使用memo还是singleton？

根据您的函数的返回类型来决定使用`@st.experimental_memo`还是`@st.experimental_singleton`。返回数据的函数应该使用`memo`，返回非数据对象的函数应该使用`singleton`。

例如：

- 数据帧计算（pandas，numpy等）：这是*数据*，使用`memo`
- 存储下载的数据：`memo`
- 计算π的n位小数：`memo`
- Tensorflow会话：这是*非数据对象*，使用`singleton`
- 数据库连接: `singleton`

### 逐步清除备忘录和单例缓存

您可以在代码中逐步清除使用`@st.experimental_memo`和`@st.experimental_singleton`装饰的函数的缓存。例如，您可以执行以下操作:

```python
@st.experimental_memo
def square(x):
    return x**2

if st.button("Clear Square"):
    # Clear square's memoized values:
    square.clear()

if st.button("Clear All"):
    # Clear values from *all* memoized functions:
    st.experimental_memo.clear()
```

按下“清除方块”按钮将清除`square()`的备忘录值。按下“清除全部”按钮将清除所有使用`@st.experimental_memo`装饰的函数的备忘录值。

总结：

- 任何使用`@st.experimental_memo`或`@st.experimental_singleton`注释的函数都会自动获得自己的`clear()`函数。
- 此外，您还可以使用 [`st.experimental_memo.clear()`](/library/api-reference/performance/st.experimental_memo.clear) 和 [`st.experimental_singleton.clear()`](/library/api-reference/performance/st.experimental_singleton.clear) 来清除所有的 memo 和 singleton 缓存。

<Note>

这些命令是**实验性的**，因此受到我们的[实验性 API 过程](/library/advanced-features/prerelease#experimental)的管理。

</Note>

这些专门的**记忆化**和**单例**命令是Streamlit演进的重要一步，有可能在2022年的某个时候完全取代`@st.cache`。

是的，今天你可以使用`@st.cache`来存储从数据库连接中获取的数据（例如Tensorflow会话，缓存长时间计算的结果，如更改pandas数据帧上的日期时间值等）。但这些是非常不同的事物，所以我们创建了两个新函数，这将使它们更加快速！💨

请通过在[Streamlit论坛](https://discuss.streamlit.io/)中测试这些命令并留下评论来帮助我们。