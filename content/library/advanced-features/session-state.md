---
title: 为应用程序添加状态
slug: /library/advanced-features/session-state
---

# 为应用程序添加状态

## 什么是状态？

我们将在浏览器标签中访问Streamlit应用程序称为**会话**。对于每个连接到Streamlit服务器的浏览器标签，都会创建一个新的会话。每当您与应用程序进行交互时，Streamlit会从头到尾重新运行您的脚本。每次重新运行都在一个空白的状态下进行：在运行之间没有共享变量。

会话状态是一种在每个用户会话中共享变量的方式。除了存储和持久化状态的能力外，Streamlit还提供了使用回调函数来操作状态的能力。会话状态还可以在[multipage app](/library/get-started/multipage-apps)中的应用之间进行持久化。

在本指南中，我们将演示如何使用**会话状态**和**回调函数**来构建一个具有状态的计数器应用程序。

有关会话状态和回调 API 的详细信息，请参阅我们的 [会话状态 API 参考指南](/library/api-reference/session-state)。

此外，您还可以观看 Streamlit 开发者倡导者 Dr. Marisa Smith 的这个会话状态基础教程视频，以便入门：

<YouTube videoId="92jUAXBmZyU" />

## 构建一个计数器

让我们将脚本命名为 `counter.py`。它初始化了一个 `count` 变量，并且有一个按钮来增加存储在 `count` 变量中的值：

```python
import streamlit as st

st.title('Counter Example')
count = 0

increment = st.button('Increment')
if increment:
    count += 1

st.write('Count = ', count)
```

无论我们在上面的应用程序中按下**_增加_**按钮多少次，`count`始终保持为1。让我们了解一下原因：

- 每次按下**_增加_**按钮时，Streamlit会从上到下重新运行`counter.py`，并且每次运行时，`count`都会被初始化为`0`。
- 连续按下**_增加_**按钮将1加到0上，因此无论我们按下**_增加_**按钮多少次，`count`都为1。

正如我们将在后面看到的那样，我们可以通过将`count`存储为会话状态变量来避免这个问题。通过这样做，我们告诉Streamlit应该在应用程序重新运行时维护存储在会话状态变量中的值。

让我们更多地了解使用会话状态的API。

### 初始化

会话状态API遵循基于字段的API，非常类似于Python字典：

```python
import streamlit as st

# Check if 'key' already exists in session_state
# If not, then initialize it
if 'key' not in st.session_state:
    st.session_state['key'] = 'value'

# Session State also supports the attribute based syntax
if 'key' not in st.session_state:
    st.session_state.key = 'value'
```

### 读取和更新

通过将项目传递给 `st.write` 来读取会话状态中的项目的值:

```python
import streamlit as st

if 'key' not in st.session_state:
    st.session_state['key'] = 'value'

# Reads
st.write(st.session_state.key)

# Outputs: value
```

通过为其分配一个值来更新会话状态中的项目：

```python
import streamlit as st

if 'key' not in st.session_state:
    st.session_state['key'] = 'value'

# Updates
st.session_state.key = 'value2'     # Attribute API
st.session_state['key'] = 'value2'  # Dictionary like API
```

如果访问了一个未初始化的变量，Streamlit会抛出异常：

```python
import streamlit as st

st.write(st.session_state['value'])

# Throws an exception!
```

![state-uninitialized-exception](/images/state_uninitialized_exception.png)

现在让我们来看一些示例，演示如何为我们的计数器应用程序添加会话状态。

### 示例1：添加会话状态

既然我们已经了解了会话状态API的用法，让我们更新我们的计数器应用程序，使用会话状态：

```python
import streamlit as st

st.title('Counter Example')
if 'count' not in st.session_state:
    st.session_state.count = 0

increment = st.button('Increment')
if increment:
    st.session_state.count += 1

st.write('Count = ', st.session_state.count)
```

正如您在上面的示例中所看到的，每次按下**_增加_**按钮时，`count`都会更新。

### 示例2：会话状态和回调

现在我们已经使用会话状态构建了一个基本的计数器应用程序，让我们进一步学习一些更复杂的内容。下一个示例将使用会话状态和回调函数。

**回调函数**：回调函数是一个在输入小部件发生更改时调用的Python函数。可以使用`on_change`（或`on_click`）、`args`和`kwargs`参数与小部件一起使用回调函数。完整的回调函数API可以在我们的[会话状态API参考指南](/library/api-reference/session-state#use-callbacks-to-update-session-state)中找到。

```python
import streamlit as st

st.title('Counter Example using Callbacks')
if 'count' not in st.session_state:
    st.session_state.count = 0

def increment_counter():
    st.session_state.count += 1

st.button('Increment', on_click=increment_counter)

st.write('Count = ', st.session_state.count)
```

现在，按下**_增加_**按钮会通过调用`increment_counter()`函数来更新计数。

### 示例3：在回调中使用args和kwargs

回调还支持使用`args`参数在小部件中传递参数：

```python
import streamlit as st

st.title('Counter Example using Callbacks with args')
if 'count' not in st.session_state:
    st.session_state.count = 0

increment_value = st.number_input('Enter a value', value=0, step=1)

def increment_counter(increment_value):
    st.session_state.count += increment_value

increment = st.button('Increment', on_click=increment_counter,
    args=(increment_value, ))

st.write('Count = ', st.session_state.count)
```

此外，我们还可以在小部件中使用`kwargs`参数来将命名参数传递给回调函数，示例如下：

```python
import streamlit as st

st.title('Counter Example using Callbacks with kwargs')
if 'count' not in st.session_state:
    st.session_state.count = 0

def increment_counter(increment_value=0):
    st.session_state.count += increment_value

def decrement_counter(decrement_value=0):
    st.session_state.count -= decrement_value

st.button('Increment', on_click=increment_counter,
	kwargs=dict(increment_value=5))

st.button('Decrement', on_click=decrement_counter,
	kwargs=dict(decrement_value=1))

st.write('Count = ', st.session_state.count)
```

### 示例 4: 表单和回调

假设我们现在不仅想要增加`count`，而且还想要记录它上次更新的时间。我们将使用回调和`st.form`来实现这一点:

```python
import streamlit as st
import datetime

st.title('Counter Example')
if 'count' not in st.session_state:
    st.session_state.count = 0
    st.session_state.last_updated = datetime.time(0,0)

def update_counter():
    st.session_state.count += st.session_state.increment_value
    st.session_state.last_updated = st.session_state.update_time

with st.form(key='my_form'):
    st.time_input(label='Enter the time', value=datetime.datetime.now().time(), key='update_time')
    st.number_input('Enter a value', value=0, step=1, key='increment_value')
    submit = st.form_submit_button(label='Update', on_click=update_counter)

st.write('Current Count = ', st.session_state.count)
st.write('Last Updated = ', st.session_state.last_updated)
```

## 高级概念

### 会话状态和小部件状态的关联

会话状态提供了在重新运行时跨变量进行存储的功能。小部件状态（即小部件的值）也存储在会话中。

为了简化操作，我们将这些信息_统一_在一个地方，即会话状态。这个便利功能使得在应用程序的任何代码中都可以非常轻松地读取或写入小部件的状态。会话状态变量使用`key`参数与小部件的值相对应。

我们通过以下示例来说明这一点。假设我们有一个应用程序，其中有一个滑块来表示摄氏温度。我们可以使用会话状态API来**设置**和**获取**温度小部件的值，如下所示：

```python
import streamlit as st

if "celsius" not in st.session_state:
    # set the initial default value of the slider widget
    st.session_state.celsius = 50.0

st.slider(
    "Temperature in Celsius",
    min_value=-100.0,
    max_value=100.0,
    key="celsius"
)

# This will get the value of the slider widget
st.write(st.session_state.celsius)
```

使用 Session State API 设置小部件值存在限制。

<Important>

Streamlit **不允许**通过 Session State API 为 `st.button` 和 `st.file_uploader` 设置小部件值。

</Important>

尝试通过 Session State API 设置 `st.button` 状态的以下示例将引发 `StreamlitAPIException` 异常：

```python
import streamlit as st

if 'my_button' not in st.session_state:
    st.session_state.my_button = True
    # Streamlit will raise an Exception on trying to set the state of button

st.button('Submit', key='my_button')
```

![state-button-exception](/images/state_button_exception.png)

### 可序列化的会话状态

序列化是指将对象或数据结构转换为可持久化和共享的格式，并允许您恢复数据的原始结构的过程。Python内置的[pickle](https://docs.python.org/3/library/pickle.html)模块可以将Python对象序列化为字节流（"pickling"），并将流反序列化为对象（"unpickling"）。

默认情况下，Streamlit的[会话状态](/library/advanced-features/session-state)允许您在会话期间持久化任何Python对象，无论对象的pickle序列化能力如何。这个属性允许您存储Python的原始数据类型，如整数、浮点数、复数和布尔值，数据帧，甚至由函数返回的[lambdas](https://docs.python.org/3/reference/expressions.html#lambda)。然而，一些执行环境可能要求序列化会话状态中的所有数据，因此在开发过程中检测不兼容性或在将来停止支持它的执行环境中可能会很有用。

为此，Streamlit提供了一个`runner.enforceSerializableSessionState` [配置选项](https://docs.streamlit.io/library/advanced-features/configuration)，当设置为`true`时，只允许在Session State中使用可pickle序列化的对象。要启用此选项，可以创建一个全局或项目配置文件，包含以下内容，或将其用作命令行标志:

```toml
# .streamlit/config.toml
[runner]
enforceSerializableSessionState = true
```

通过"_pickle-serializable_"，我们指的是调用`pickle.dumps(obj)`不应该引发[`PicklingError`](https://docs.python.org/3/library/pickle.html#pickle.PicklingError)异常。当启用配置选项时，将不可序列化的数据添加到会话状态应该会引发异常。例如，

```python
import streamlit as st

def unserializable_data():
		return lambda x: x

#👇 results in an exception when enforceSerializableSessionState is on
st.session_state.unserializable = unserializable_data()
```

![UnserializableSessionStateError](/images/unserializable-session-state-error.png)

### 注意事项和限制

在使用会话状态时，请记住以下一些限制：

- 会话状态存在于标签页打开并连接到Streamlit服务器的时间内。一旦关闭标签页，存储在会话状态中的所有内容都会丢失。
- 会话状态不会持久化。如果Streamlit服务器崩溃，那么存储在会话状态中的所有内容都会被清除。
- 有关会话状态 API 的注意事项和限制，请参阅 [API 限制](/library/api-reference/session-state#caveats-and-limitations)。
