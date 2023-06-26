---
slug: /library/advanced-features/st.cache
title: Optimize performance with st.cache
---

<弃用>

`st.cache`在1.18.0版本中已被弃用。请改用[`st.cache_data`](/library/api-reference/performance/st.cache_data)或[`st.cache_resource`](/library/api-reference/performance/st.cache_resource)。了解更多信息请参阅[缓存](/library/advanced-features/caching)。

</弃用>

# 使用st.cache优化性能

Streamlit提供了一个缓存机制，使得您的应用在从网络加载数据、操作大型数据集或执行昂贵的计算时保持高性能。这是通过[`@st.cache`](/library/api-reference/performance/st.cache)装饰器实现的。

当您使用[`@st.cache`](/library/api-reference/performance/st.cache)装饰器标记一个函数时，它告诉Streamlit在每次调用该函数时需要检查一些事情：

1. 您调用函数时使用的输入参数
2. 函数中使用的任何外部变量的值
3. 函数的主体
4. 缓存函数内部使用的任何函数的主体

如果这是Streamlit第一次看到这四个具有这些确切值、确切组合和顺序的组件，它将运行函数并将结果存储在本地缓存中。然后，下次调用缓存函数时，如果这些组件都没有发生变化，Streamlit将直接跳过执行函数，而是返回先前存储在缓存中的输出。

Streamlit跟踪这些组件的变化方式是通过哈希。将缓存视为一个内存中的键值存储，其中键是所有上述内容的哈希值，值是实际的输出对象通过引用传递。

最后，[`@st.cache`](/library/api-reference/performance/st.cache)支持配置缓存行为的参数。您可以在我们的[API参考](/library/api-reference)中找到更多信息。

让我们看一下一些示例，以说明在Streamlit应用程序中缓存是如何工作的。

## 示例1：基本用法

首先，让我们看一个示例应用程序，其中包含一个执行昂贵且耗时的计算的函数。如果没有缓存，每次刷新应用程序时都会重新运行该函数，导致用户体验较差。将以下代码复制到一个新的应用程序中，自己试一试：

```python
import streamlit as st
import time

def expensive_computation(a, b):
    time.sleep(2)  # 👈 This makes the function take 2s to run
    return a * b

a = 2
b = 21
res = expensive_computation(a, b)

st.write("Result:", res)
```

尝试按下 **R** 键重新运行应用程序，并注意结果显示所需的时间。这是因为每次应用程序运行时都会重新执行 `expensive_computation(a, b)`。这并不是一个很好的体验。

让我们添加 [`@st.cache`](/library/api-reference/performance/st.cache) 装饰器：

```python
import streamlit as st
import time

@st.cache  # 👈 Added this
def expensive_computation(a, b):
    time.sleep(2)  # This makes the function take 2s to run
    return a * b

a = 2
b = 21
res = expensive_computation(a, b)

st.write("Result:", res)
```

现在再次运行应用程序，您会注意到每次按下R重新运行时速度都会更快。为了了解发生了什么，让我们在函数内部添加一个st.write：

```python
import streamlit as st
import time

@st.cache(suppress_st_warning=True)  # 👈 Changed this
def expensive_computation(a, b):
    # 👇 Added this
    st.write("Cache miss: expensive_computation(", a, ",", b, ") ran")
    time.sleep(2)  # This makes the function take 2s to run
    return a * b

a = 2
b = 21
res = expensive_computation(a, b)

st.write("Result:", res)
```

现在当您重新运行应用程序时，第一次运行时会出现“Cache miss”文本，但在随后的运行中不会出现。这是因为缓存函数只会执行一次，之后每次都会命中缓存。

<注意>

您可能已经注意到，我们在`@st.cache`装饰器中添加了`suppress_st_warning`关键字。这是因为上面的缓存函数本身使用了Streamlit的命令（在这种情况下是`st.write`），当Streamlit看到这个命令时，会显示一个警告，告诉您的命令只会在缓存未命中时执行。通常情况下，当您看到这个警告时，意味着代码中可能存在错误。然而，在我们的例子中，我们使用`st.write`命令来演示缓存未命中的情况，所以Streamlit警告的行为正是我们想要的。因此，我们传入`suppress_st_warning=True`来关闭这个警告。

</Note>

## 示例2: 当函数参数发生变化时

在不停止之前的应用服务器的情况下，让我们修改缓存函数的一个参数:

```python
import streamlit as st
import time

@st.cache(suppress_st_warning=True)
def expensive_computation(a, b):
    st.write("Cache miss: expensive_computation(", a, ",", b, ") ran")
    time.sleep(2)  # This makes the function take 2s to run
    return a * b

a = 2
b = 210  # 👈 Changed this
res = expensive_computation(a, b)

st.write("Result:", res)
```

现在，当您第一次重新运行应用程序时，会出现缓存未命中的情况。这可以通过显示“缓存未命中”文本并且应用程序需要2秒才能完成运行来证明。之后，如果您按下**R**键重新运行，它将始终是缓存命中。也就是说，不会显示此类文本，应用程序再次变得快速。

这是因为Streamlit会注意到参数**a**和**b**何时发生变化，并确定是否应重新执行和重新缓存该函数。

## 示例3: 当函数主体发生变化时

在不停止和重新启动您的Streamlit服务器的情况下，让我们从我们的应用程序中删除小部件，并通过将`+ 1`添加到返回值来修改函数的代码。

```python
import streamlit as st
import time

@st.cache(suppress_st_warning=True)
def expensive_computation(a, b):
    st.write("Cache miss: expensive_computation(", a, ",", b, ") ran")
    time.sleep(2)  # This makes the function take 2s to run
    return a * b + 1  # 👈 Added a +1 at the end here

a = 2
b = 210
res = expensive_computation(a, b)

st.write("Result:", res)
```

第一次运行是“缓存未命中”，但是当您按下 **R** 键后，每次运行都是缓存命中。这是因为在第一次运行时，Streamlit检测到函数体发生了变化，重新运行了函数，并将结果放入缓存中。

<提示>

如果您将函数改回原样，则结果将已经存在于Streamlit缓存中，来自上一次运行。试试看吧！

</提示>

## 示例4：当内部函数发生变化时

让我们使我们的缓存函数在内部依赖于另一个函数：

```python
import streamlit as st
import time

def inner_func(a, b):
    st.write("inner_func(", a, ",", b, ") ran")
    return a * b

@st.cache(suppress_st_warning=True)
def expensive_computation(a, b):
    st.write("Cache miss: expensive_computation(", a, ",", b, ") ran")
    time.sleep(2)  # This makes the function take 2s to run
    return inner_func(a, b) + 1

a = 2
b = 210
res = expensive_computation(a, b)

st.write("Result:", res)
```

您看到的是通常情况下的操作：

1. 第一次运行会导致缓存未命中。
2. 每次后续的运行都会导致缓存命中。

但现在让我们尝试修改`inner_func()`函数：

```python
import streamlit as st
import time

def inner_func(a, b):
    st.write("inner_func(", a, ",", b, ") ran")
    return a ** b  # 👈 Changed the * to ** here

@st.cache(suppress_st_warning=True)
def expensive_computation(a, b):
    st.write("Cache miss: expensive_computation(", a, ",", b, ") ran")
    time.sleep(2)  # This makes the function take 2s to run
    return inner_func(a, b) + 1

a = 2
b = 21
res = expensive_computation(a, b)

st.write("Result:", res)
```

即使`inner_func()`没有被注解为[`@st.cache`](/library/api-reference/performance/st.cache)，当我们编辑其代码体时，会在外部的`expensive_computation()`中引发"Cache miss"。

这是因为Streamlit总是遍历您的代码及其依赖项，以验证缓存值是否仍然有效。这意味着在开发应用程序时，您可以自由地编辑代码，而不必担心缓存。无论您对应用程序进行了哪些更改，Streamlit都会做正确的事情！

Streamlit还聪明地只遍历属于您应用程序的依赖项，并跳过来自已安装的Python库的任何依赖项。

## 示例5：使用缓存来加速您的应用程序在用户之间的传输

回到我们的原始函数，让我们添加一个小部件来控制 `b` 的值：

```python
import streamlit as st
import time

@st.cache(suppress_st_warning=True)
def expensive_computation(a, b):
    st.write("Cache miss: expensive_computation(", a, ",", b, ") ran")
    time.sleep(2)  # This makes the function take 2s to run
    return a * b

a = 2
b = st.slider("Pick a number", 0, 10)  # 👈 Changed this
res = expensive_computation(a, b)

st.write("Result:", res)
```

您将看到以下内容：

- 如果您将滑块滑动到Streamlit之前未见过的数字，再次运行时会出现缓存未命中。当然，每次使用相同的数字重新运行时都会命中缓存。
- 如果您将滑块滑动回Streamlit之前见过的数字，缓存将被命中，应用程序将如预期般快速运行。

在计算机科学术语中，这里发生的是[`@st.cache`](/library/api-reference/performance/st.cache)正在对`expensive_computation(a, b)`进行[memoization](https://en.wikipedia.org/wiki/Memoization)。

但是现在让我们再进一步！尝试以下操作：

1. 将滑块移动到一个之前未尝试过的数字，例如9。
2. 通过在另一个浏览器标签中打开指向您的Streamlit应用的地址（通常为http://localhost:8501），来假装自己是另一个用户。
3. 在新标签中，将滑块移动到9。

注意这实际上是一个缓存命中！也就是说，即使在第二个选项卡中的用户在此之前从未将滑块移动到9，您实际上不会看到"Cache miss"文本。

这是因为Streamlit缓存是全局的，适用于所有用户。因此，每个人都对其他人的性能有所贡献。

## 示例6：修改缓存值

正如在[概述](#optimize-performance-with-stcache)部分中提到的，Streamlit缓存通过引用存储项目。这样可以使Streamlit缓存支持Python没有进行内存管理的结构，例如TensorFlow对象。然而，这也可能导致意外行为-这就是为什么Streamlit有一些检查来引导开发人员朝正确的方向发展。现在让我们来看看这些检查。

让我们编写一个应用程序，其中包含一个返回可变对象的缓存函数，然后让我们对该对象进行修改:

```python
import streamlit as st
import time

@st.cache(suppress_st_warning=True)
def expensive_computation(a, b):
    st.write("Cache miss: expensive_computation(", a, ",", b, ") ran")
    time.sleep(2)  # This makes the function take 2s to run
    return {"output": a * b}  # 👈 Mutable object

a = 2
b = 21
res = expensive_computation(a, b)

st.write("Result:", res)

res["output"] = "result was manually mutated"  # 👈 Mutated cached value

st.write("Mutated result:", res)
```

当您第一次运行此应用程序时，您应该在屏幕上看到三条消息：

- 缓存未命中（...）
- 结果：{output: 42}
- 变异结果：{output: "result was manually mutated"}

没有什么意外的。但是现在请注意当您重新运行应用程序时会发生什么（即按下 **R**）：

- 结果：{output: "result was manually mutated"}
- 变异结果：{output: "result was manually mutated"}
- 缓存对象发生变异（...）

那么问题是什么呢？

这里发生的情况是Streamlit通过引用缓存了输出`res`。当您在缓存函数之外修改了`res["output"]`时，无意中修改了缓存。这意味着对`expensive_computation(2, 21)`的每次后续调用都会返回错误的值！

由于这种行为通常不是您所期望的，Streamlit试图提供帮助，并显示一条警告，以及一些关于如何修复代码的想法。

在这种特定情况下，修复方法就是不要在缓存函数外部修改`res["output"]`。实际上，我们没有充分的理由这样做！另一种解决方法是使用`res = deepcopy(expensive_computation(2, 21))`来克隆结果值。请查看标题为[修复缓存问题](/knowledge-base/using-streamlit/caching-issues)的部分，了解更多关于这些方法和更多的信息。

## 高级缓存

在[caching](/library/advanced-features/caching)中，你学习了Streamlit的缓存功能，可以通过[`@st.cache`](/library/api-reference/performance/st.cache)装饰器来访问。在本文中，您将了解Streamlit的缓存功能是如何实现的，以便您可以使用它来提高Streamlit应用程序的性能。

缓存是一个键值存储，其中键是通过以下方式生成的哈希值：

1. 你调用函数时的输入参数
1. 函数中使用的任何外部变量的值
2. 函数的主体
3. 用于缓存函数内部使用的任何函数的主体

而该值是一个元组，包含以下内容：

- 缓存输出
- 缓存输出的哈希值（很快您就会明白原因）

对于键和输出哈希值，Streamlit使用了一种专门的哈希函数，它知道如何遍历代码，哈希特殊对象，并且可以通过用户来[自定义其行为](#the-hash_funcs-parameter)。

例如，当使用[`@st.cache`](/library/api-reference/performance/st.cache)修饰的函数`expensive_computation(a, b)`以`a=2`和`b=21`执行时，Streamlit会执行以下操作：

1. 计算缓存键
2. 如果在缓存中找到该键，则：
   - 提取先前缓存的（输出，输出哈希）元组。
   - 执行**输出突变检查**，其中计算一个新的输出哈希并与存储的`output_hash`进行比较。
     - 如果两个哈希值不同，则显示**缓存对象已更改**的警告。（注意：设置 `allow_output_mutation=True` 可禁用此步骤）。
1. 如果缓存中不存在输入键，则：
   - 执行缓存函数（即 `output = expensive_computation(2, 21)`）。
   - 从函数的输出计算 `output_hash`。
   - 将 `key → (output, output_hash)` 存储在缓存中。
1. 返回输出结果。

如果遇到错误，将会抛出异常。如果在哈希密钥或输出时发生错误，则会抛出`UnhashableTypeError`错误。如果遇到任何问题，请参阅[修复缓存问题](/knowledge-base/using-streamlit/caching-issues)。

### hash_funcs参数

如上所述，Streamlit的缓存功能依赖于哈希算法来计算缓存对象的键，并检测缓存结果中的意外变化。

为了增加表达能力，Streamlit允许您使用`hash_funcs`参数覆盖此哈希过程。假设您定义了一个名为`FileReference`的类型，它指向文件系统中的一个文件:

```python
class FileReference:
    def __init__(self, filename):
        self.filename = filename


@st.cache
def func(file_reference):
    ...
```

默认情况下，Streamlit通过递归遍历自定义类（如`FileReference`）的结构来进行哈希。在这种情况下，它的哈希是文件名属性的哈希。只要文件名不变，哈希值就会保持不变。

但是，如果您希望哈希检查文件的修改时间而不仅仅是文件名呢？这可以通过[`@st.cache`](/library/api-reference/performance/st.cache)的`hash_funcs`参数实现：

```python
class FileReference:
    def __init__(self, filename):
        self.filename = filename

def hash_file_reference(file_reference):
    filename = file_reference.filename
    return (filename, os.path.getmtime(filename))

@st.cache(hash_funcs={FileReference: hash_file_reference})
def func(file_reference):
    ...
```

此外，您还可以通过文件内容对`FileReference`对象进行哈希处理：

```python
class FileReference:
    def __init__(self, filename):
        self.filename = filename

def hash_file_reference(file_reference):
    with open(file_reference.filename) as f:
      return f.read()

@st.cache(hash_funcs={FileReference: hash_file_reference})
def func(file_reference):
    ...
```

<注意>

由于Streamlit的哈希函数是递归的，您无需在`hash_file_reference`中哈希文件内容。相反，您可以返回一个原始类型，例如文件的内容，然后Streamlit的内部哈希计算器将从中计算实际哈希值。

</注意>

### 典型的哈希函数

虽然可以编写自定义的哈希函数，但让我们来看一下Python提供的一些开箱即用的工具。下面是一些哈希函数及其适用场景的列表。

Python的[`id`](https://docs.python.org/3/library/functions.html#id)函数 | [示例](#example-1-pass-a-database-connection-around)

- 速度：快速
- 使用场景: 如果您要对单例对象进行哈希处理，比如打开的数据库连接或TensorFlow会话。这些对象只会被实例化一次，无论您的脚本运行多少次。

`lambda _: None` | [示例](#example-2-turn-off-hashing-for-a-specific-type)

- 速度: 快
- 使用场景: 如果您想要关闭对此类型的哈希处理。这对于您知道对象不会改变时非常有用。

Python的[`hash()`](https://docs.python.org/3/library/functions.html#hash)函数 | [示例](#example-3-use-pythons-hash-function)

- 速度：根据被缓存对象的大小可能会较慢
- 使用场景：如果Python已经知道如何正确散列此类型。

自定义哈希函数 | [示例](#the-hash_funcs-parameter)

- 速度：不适用
- 使用场景：如果您想要覆盖Streamlit如何散列特定类型。

### 示例1：传递数据库连接

假设我们想要打开一个数据库连接，可以在多次运行Streamlit应用程序时重复使用。为此，您可以利用缓存对象存储的引用来自动初始化和重用连接：

```python
@st.cache(allow_output_mutation=True)
def get_database_connection():
    return db.get_connection()
```

只需要3行代码，数据库连接就会被创建一次并存储在缓存中。然后，每次调用`get_database_connection`时，已创建的连接对象会自动重用。换句话说，它成为一个单例。

<提示>

使用`allow_output_mutation=True`标志来抑制不可变性检查。这样可以防止Streamlit尝试对输出连接进行哈希处理，并在此过程中关闭Streamlit的变异警告。

</提示>

如果您想编写一个接收数据库连接作为输入的函数，您可以使用`hash_funcs`：

```python
@st.cache(hash_funcs={DBConnection: id})
def get_users(connection):
    # Note: We assume that connection is of type DBConnection.
    return connection.execute_sql('SELECT * from Users')
```

在这里，我们使用Python内置的`id`函数，因为连接对象是通过`get_database_connection`函数从Streamlit缓存中获取的。这意味着每次传递的都是同一个连接实例，因此它的id始终相同。但是，如果您恰好有一个指向完全不同数据库的第二个连接对象，仍然可以安全地将其传递给`get_users`，因为它的id保证与第一个id不同。

这些设计模式适用于任何时候当您有一个指向外部资源的对象，例如数据库连接或Tensorflow会话。

### 示例2：为特定类型关闭哈希

您可以通过为特定类型提供一个自定义哈希函数来完全关闭哈希。您可能这样做的一个原因是为了避免对大型、慢速哈希的对象进行哈希，而您知道这些对象不会发生变化。例如:

```python
@st.cache(hash_funcs={pd.DataFrame: lambda _: None})
def func(huge_constant_dataframe):
    ...
```

当Streamlit遇到这种类型的对象时，无论它正在查看哪个`FooType`的实例，它总是将对象转换为`None`。这意味着所有实例都被哈希为相同的值，这有效地取消了哈希机制。

### 示例3：使用Python的hash()函数

有时候，您可能想要使用Python的默认哈希算法，而不是Streamlit的。例如，也许您遇到了Streamlit无法哈希的类型，但是使用Python内置的`hash()`函数是可哈希的：

```python
@st.cache(hash_funcs={FooType: hash})
def func(...):
    ...
```