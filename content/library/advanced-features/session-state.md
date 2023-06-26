---
slug: /library/advanced-features/session-state
title: Add statefulness to apps
---

# 为应用程序添加状态性

## 什么是状态？

我们将在浏览器标签中访问Streamlit应用程序定义为一个**会话**。对于连接到Streamlit服务器的每个浏览器标签，都会创建一个新的会话。每当与应用程序进行交互时，Streamlit会从头到尾重新运行您的脚本。每次重新运行都会在一个空白状态下进行：没有变量在运行之间共享。

会话状态是一种在每个用户会话中共享变量的方式。除了能够存储和持久化状态外，Streamlit还提供了使用回调函数来操作状态的能力。会话状态还会在多页应用程序中持久化，可以跨应用进行共享。

在本指南中，我们将演示如何使用**会话状态**和**回调函数**来构建一个有状态的计数器应用程序。

有关Session State和Callbacks API的详细信息，请参阅我们的[Session State API参考指南](/library/api-reference/session-state)。

此外，查看Streamlit开发者大使Dr. Marisa Smith的Session State基础教程视频，以开始使用：

<YouTube videoId="92jUAXBmZyU" />

## 构建一个计数器

让我们将脚本命名为`counter.py`。它初始化一个`count`变量，并有一个按钮来递增存储在`count`变量中的值：

```python
import streamlit as st

st.title('Counter Example')
count = 0

increment = st.button('Increment')
if increment:
    count += 1

st.write('Count = ', count)
```

无论我们在上述应用程序中按下多少次“增加”按钮，`count`始终保持为1。让我们理解为什么：

- 每当我们按下“增加”按钮时，Streamlit会从上到下重新运行`counter.py`，并且每次运行时，`count`都会被初始化为`0`。
- 连续按下“增加”后，将1添加到0，因此无论我们按下多少次“增加”，`count`都等于1。

正如我们稍后将看到的那样，我们可以通过将`count`存储为Session State变量来避免此问题。通过这样做，我们告诉Streamlit应该在应用程序重新运行时保持存储在Session State变量中的值不变。

让我们更深入地了解如何使用Session State的API。

### 初始化

Session State API遵循基于字段的API，这与Python字典非常相似：

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

通过将要读取的项目传递给 `st.write` ，可以读取Session State中的项目的值：

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

现在让我们看一些示例，说明如何将会话状态添加到我们的计数器应用程序中。

### 示例1：添加会话状态

既然我们已经掌握了会话状态API，让我们更新我们的计数器应用程序以使用会话状态：

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

现在，我们已经使用会话状态构建了一个基本的计数器应用程序，让我们继续进行一些更复杂的操作。下一个示例使用了带有会话状态的回调函数。

**回调函数**：回调函数是一个在输入小部件发生变化时被调用的Python函数。可以使用`on_change`（或`on_click`）、`args`和`kwargs`参数与小部件一起使用回调函数。完整的回调函数 API 可在我们的[会话状态 API 参考指南](/library/api-reference/session-state#use-callbacks-to-update-session-state)中找到。

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

现在，按下 **_Increment_** 按钮会通过调用 `increment_counter()` 函数来每次更新计数。

### 示例3：在回调函数中使用args和kwargs

回调函数还支持使用 `args` 参数在小部件中传递参数:

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

另外，我们还可以在小部件中使用`kwargs`参数，将命名参数传递给回调函数，如下所示：

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

### 示例 4：表单和回调函数

假设我们不仅想递增`count`，还想记录它的最后更新时间。我们可以使用回调函数和`st.form`来实现这个功能：

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

### 会话状态与小部件状态的关联

会话状态提供了在重新运行时跨变量存储的功能。小部件状态（即小部件的值）也存储在会话中。

为了简化起见，我们将此信息统一放在一个地方，即会话状态。这个便利的功能使得在应用程序的任何代码中读取或写入小部件的状态变得非常容易。会话状态变量通过 `key` 参数来映射小部件的值。

我们以以下示例来说明。假设我们有一个应用程序，其中有一个滑块来表示摄氏温度。我们可以使用会话状态API来**设置**和**获取**温度小部件的值，如下所示：

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

使用Session State API设置小部件值存在限制。

<重要提示>

Streamlit **不允许**通过Session State API为`st.button`和`st.file_uploader`设置小部件值。

</重要提示>

尝试通过Session State API设置`st.button`状态的以下示例将引发`StreamlitAPIException`异常：

```python
import streamlit as st

if 'my_button' not in st.session_state:
    st.session_state.my_button = True
    # Streamlit will raise an Exception on trying to set the state of button

st.button('Submit', key='my_button')
```

![state-button-exception](/images/state_button_exception.png)

### 可序列化的会话状态

序列化是指将对象或数据结构转换为可以持久化和共享的格式，并允许您恢复数据的原始结构。Python内置的[pickle](https://docs.python.org/3/library/pickle.html)模块可以将Python对象序列化为字节流（"pickling"），并将流反序列化为对象（"unpickling"）。

默认情况下，Streamlit的[会话状态](/library/advanced-features/session-state)允许您在会话期间持久保存任何Python对象，无论对象是否可被pickle序列化。这个特性允许您存储Python的原始类型，如整数、浮点数、复数和布尔值，数据帧，甚至由函数返回的[lambdas](https://docs.python.org/3/reference/expressions.html#lambda)。然而，一些执行环境可能需要序列化会话状态中的所有数据，因此在开发过程中或将来执行环境停止支持时检测到不兼容性可能是有用的。

为此，Streamlit提供了一个`runner.enforceSerializableSessionState` [配置选项](https://docs.streamlit.io/library/advanced-features/configuration)，当设置为`true`时，只允许在Session State中使用可pickle序列化的对象。要启用此选项，可以创建一个全局或项目配置文件，并使用以下内容，或将其用作命令行标志:

```toml
# .streamlit/config.toml
[runner]
enforceSerializableSessionState = true
```

"_pickle-serializable_" 指的是调用 `pickle.dumps(obj)` 不应该引发 [`PicklingError`](https://docs.python.org/3/library/pickle.html#pickle.PicklingError) 异常。当启用配置选项时，将不可序列化的数据添加到会话状态中应该引发异常。例如，

```python
import streamlit as st

def unserializable_data():
		return lambda x: x

#👇 results in an exception when enforceSerializableSessionState is on
st.session_state.unserializable = unserializable_data()
```

![UnserializableSessionStateError](/images/unserializable-session-state-error.png)

### 注意事项和限制

在使用会话状态时，请记住以下限制：

- 会话状态仅在选项卡打开且连接到Streamlit服务器时存在。一旦关闭选项卡，会话状态中存储的所有内容都会丢失。
- 会话状态不会持久保存。如果Streamlit服务器崩溃，则会话状态中存储的所有内容都会被清除。
- 有关会话状态API的注意事项和限制，请参阅[API限制](/library/api-reference/session-state#caveats-and-limitations)。