---
标题：实验性缓存原语
链接：/library/advanced-features/experimental-cache-primitives

<Deprecation>

本页面描述的实验性缓存原语已在版本1.18.0中弃用。请改用[`st.cache_data`](/library/api-reference/performance/st.cache_data)或[`st.cache_resource`](/library/api-reference/performance/st.cache_resource)。了解更多信息，请参阅[Caching](/library/advanced-features/caching)。

</Deprecation>

# 实验性缓存原语

## 概述

Streamlit的独特执行模型是使其使用起来愉快的一部分：您的代码从上到下执行，就像一个简单的脚本一样，适用于每次交互。不需要考虑模型、视图、控制器或其他类似的东西。

每当您的代码重新执行时，一个名为[`@st.cache`](/library/api-reference/performance/st.cache)的装饰器会提供一个缓存机制，该装饰器是一种强大的记忆化和状态存储功能，使您的应用程序即使在从网络加载数据、操作大型数据集或执行昂贵计算时也能保持高性能。

然而，我们发现[`@st.cache`](/library/advanced-features/caching)难以使用且不够快速。您可能会遇到诸如`InternalHashError`或`UnhashableTypeError`之类的晦涩错误。或者您需要理解诸如[`hash_funcs`](/library/advanced-features/caching#the-hash_funcs-parameter)和[`allow_output_mutation`](/library/advanced-features/caching#example-1-pass-a-database-connection-around)之类的概念。

我们的解决方案包括两个新的原语: [**`st.experimental_memo`**](/library/api-reference/performance/st.experimental_memo) 和 [**`st.experimental_singleton`**](/library/api-reference/performance/st.experimental_singleton)。它们在概念上更简单，速度也更快。在我们对缓存大型数据帧的一些内部测试中，`@st.experimental_memo` 的性能比 `@st.cache` 高出一个数量级。这是超过10倍的速度！🚀

让我们来看看这两个实验性API的用例，以及它们如何比`@st.cache`更具重要性的改进。

问题

`@st.cache`用于以下用例：

1. 存储给定不同类型输入的计算结果。在计算机科学文献中，这被称为[记忆化](https://en.wikipedia.org/wiki/Memoization)。
2. 在Streamlit服务器的生命周期内，仅初始化一个对象，并在每次重新运行时重用该实例。这被称为[**单例模式**](https://en.wikipedia.org/wiki/Singleton_pattern)。
3. 存储全局状态，以便在多个Streamlit会话中共享和修改（由于Streamlit是多线程的，因此需要特别注意线程安全性）。

由于`@st.cache`试图在单个统一的API下涵盖太多用例，所以它既慢又复杂。

## 解决方案

虽然`@st.cache`尝试同时解决两个非常不同的问题（缓存数据和共享全局单例对象），但这些新的原语通过将问题分割成两个不同的API来简化了问题。因此，它们更快也更简单。

### `@st.experimental_memo`

使用[`@st.experimental_memo`](/library/api-reference/performance/st.experimental_memo)来存储昂贵的计算，这种计算可以在传统意义上被“缓存”或“记忆化”。它几乎与现有的`@st.cache`具有完全相同的API，因此您通常可以将一个替换为另一个。

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

- 与`@st.cache`不同，此函数通过值而不是引用返回缓存项。这意味着您不再需要担心意外更改缓存中存储的项。在内部，这是通过使用Python的`pickle()`函数对缓存值进行序列化/反序列化来实现的。
- 尽管这个示例使用了自定义的哈希解决方案来生成缓存键（比如 `@st.cache`），但它**_不_**使用 `hash_funcs` 作为无法哈希参数的逃生通道。相反，我们允许您通过在参数前加下划线来忽略无法哈希的参数（例如数据库连接）。

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

[`@st.experimental_singleton`](/library/api-reference/performance/st.experimental_singleton) 是一个在Streamlit应用程序的所有会话中共享的键值存储。它非常适合在会话之间存储重量级的单例对象（如TensorFlow/Torch/Keras会话和/或数据库连接）。

使用示例：

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

#### 与 `@st.cache` 的比较:

- 像 `@st.cache` 一样，**这个装饰器也是通过引用返回项目。**
- 您可以返回任何类型的对象，包括不可序列化的对象。
- 与 `@st.cache` 不同，这个装饰器没有额外的逻辑来检查您是否意外地修改了缓存的对象。那些逻辑很慢，而且会产生令人困惑的错误信息。因此，我们希望通过将这个装饰器称为“singleton”，来引导您采取正确的行为。
- 这不符合计算图规范。
- 你不需要担心`hash_funcs`！只需在参数前加下划线以忽略它们。

<警告>

单例对象可以被连接到您的应用程序的每个用户并发使用，_您需要确保`@st.singleton`对象是线程安全的_。（大多数您想要放在`@st.singleton`注释中的对象可能已经是安全的，但您应该进行验证。）

</警告>

### 选择使用memo还是singleton？

根据函数的返回类型来决定使用`@st.experimental_memo`还是`@st.experimental_singleton`。返回数据的函数应该使用`memo`，返回非数据对象的函数应该使用`singleton`。

例如：

- 数据帧计算（pandas，numpy等）：这是数据，使用`memo`
- 存储下载的数据：`memo`
- 计算π的n位小数：`memo`
- Tensorflow会话：这是非数据对象，使用`singleton`
- 数据库连接: `singleton`

### 程序化地清除备忘录和单例缓存

您可以在代码中清除使用`@st.experimental_memo`和`@st.experimental_singleton`装饰的函数的缓存。例如，您可以执行以下操作:

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

按下"清除方形"按钮将清除`square()`的记忆化值。按下"清除全部"按钮将清除所有使用`@st.experimental_memo`装饰的函数的记忆化值。

总结如下：

- 使用`@st.experimental_memo`或`@st.experimental_singleton`注释的任何函数都会自动获得自己的`clear()`函数。
- 另外，您还可以使用[`st.experimental_memo.clear()`](/library/api-reference/performance/st.experimental_memo.clear)和[`st.experimental_singleton.clear()`](/library/api-reference/performance/st.experimental_singleton.clear)来清除所有的备忘录和单例缓存。

<注意>

这些命令是**实验性的**，因此受到我们的[实验性API流程](/library/advanced-features/prerelease#experimental)的管理。

</注意>

这些专门的**memoization**和**singleton**命令代表了Streamlit发展的重要一步，有可能在2022年的某个时刻完全取代`@st.cache`。

是的，今天你可以使用`@st.cache`来存储从数据库连接中获取的数据（例如Tensorflow会话，缓存长时间计算的结果，如更改pandas数据帧上的日期时间值等）。但这些是非常不同的事情，所以我们创建了两个新的函数，将使它们更快！💨

请通过在[Streamlit论坛](https://discuss.streamlit.io/)中测试这些命令并留下评论来帮助我们。
