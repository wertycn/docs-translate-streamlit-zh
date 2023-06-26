---
description: st.experimental_memo is used to memoize function executions.
slug: /library/api-reference/performance/st.experimental_memo
title: st.experimental_memo
---

<重要>

这是一个实验性功能。实验性功能及其API可能随时更改或移除。要了解更多信息，请点击[这里](/library/advanced-features/prerelease#experimental-features)。

</重要>

`<Autofunction function="streamlit.experimental_memo" deprecated={true} deprecatedText="<code>st.experimental_memo</code> was deprecated in version 1.18.0. Use <a href='/library/api-reference/performance/st.cache_data'><code>st.cache_data</code></a> instead. Learn more in <a href='/library/advanced-features/caching'>Caching</a>."/>

当前持久化备忘录缓存不支持TTL。如果指定了`persist`，则`ttl`将被忽略：`

```python
import streamlit as st

@st.experimental_memo(ttl=60, persist="disk")
def load_data():
    return 42

st.write(load_data())
```

并且会在您的终端上记录一个警告：

```bash
streamlit run app.py

  You can now view your Streamlit app in your browser.
  Local URL: http://localhost:8501
  Network URL: http://192.168.1.1:8501

2022-09-22 13:35:41.587 The memoized function 'load_data' has a TTL that will be ignored. Persistent memo caches currently don't support TTL.
```

### 在带有缓存装饰的函数中回放静态 `st` 元素

使用 `@st.experimental_memo` 装饰的函数可以包含静态的 `st` 元素。当执行一个带有缓存装饰的函数时，我们记录产生的元素和阻塞的消息，因此即使函数的执行被跳过，因为结果已经被缓存，这些元素也会出现在应用程序中。

在下面的示例中，使用`@st.experimental_memo`装饰器来缓存`load_data`函数的执行结果，该函数返回一个pandas DataFrame。注意，缓存的函数还包含一个`st.area_chart`命令，当函数被跳过执行时，该命令将被重新执行，因为结果已经被缓存。

```python
import numpy as np
import pandas as pd
import streamlit as st

@st.experimental_memo
def load_data(rows):
    chart_data = pd.DataFrame(
        np.random.randn(rows, 10),
        columns=["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"],
    )
    # Contains a static element st.area_chart
    st.area_chart(chart_data) # This will be recorded and displayed even when the function is skipped
    return chart_data

df = load_data(20)
st.dataframe(df)
```

`cache-decorated`函数中支持的静态`st`元素包括：

- `st.alert`
- `st.altair_chart`
- `st.area_chart`
- `st.audio`
- `st.bar_chart`
- `st.ballons`
- `st.bokeh_chart`
- `st.caption`
- `st.code`
- `st.components.v1.html`
- `st.components.v1.iframe`
- `st.container`
- `st.dataframe`
- `st.echo`
- `st.empty`
- `st.error`
- `st.exception`
- `st.expander`
- `st.experimental_get_query_params`
- `st.experimental_set_query_params`
- `st.form`
- `st.form_submit_button`
- `st.graphviz_chart`
- `st.help`
- `st.image`
- `st.info`
- `st.json`
- `st.latex`
- `st.line_chart`
- `st.markdown`
- `st.metric`
- `st.plotly_chart`
- `st.progress`
- `st.pydeck_chart`
- `st.snow`
- `st.spinner`
- `st.success`
- `st.table`
- `st.text`
- `st.vega_lite_chart`
- `st.video`
- `st.warning`

### 在带有缓存装饰的函数中重新播放输入小部件

除了静态元素之外，使用`@st.experimental_memo`装饰的函数还可以包含[input widgets](/library/api-reference/widgets)！回放输入小部件默认是禁用的。要启用它，您可以将`@st.experimental_memo`的`experimental_allow_widgets`参数设置为`True`。下面的示例启用了小部件回放，并展示了在缓存装饰函数内使用复选框小部件的用法。

```python
import streamlit as st

# Enable widget replay
@st.experimental_memo(experimental_allow_widgets=True)
def func():
    # Contains an input widget
    st.checkbox("Works!")

func()
```

如果缓存修饰的函数中包含输入小部件，但`experimental_allow_widgets`设置为`False`或未设置，Streamlit将抛出一个`CachedStFunctionWarning`警告，如下所示：

```python
import streamlit as st

# Widget replay is disabled by default
@st.experimental_memo
def func():
    # Streamlit will throw a CachedStFunctionWarning
    st.checkbox("Doesn't work")

func()
```

![缓存函数警告小部件](/images/cached-st-function-warning-widgets.png)

### 小部件重放的工作原理

让我们揭开缓存修饰函数中小部件重放的神秘面纱，并获得概念上的理解。小部件值被视为函数的附加输入，并用于确定是否应该执行函数。考虑以下示例：

```python
import streamlit as st

@st.experimental_memo(experimental_allow_widgets=True)
def plus_one(x):
    y = x + 1
    if st.checkbox("Nuke the value 💥"):
        st.write("Value was nuked, returning 0")
        y = 0
    return y

st.write(plus_one(2))
```

`plus_one`函数接受一个整数 `x` 作为输入，并返回 `x + 1`。该函数还包含一个复选框小部件，用于“清除” `x` 的值。即`plus_one`的返回值取决于复选框的状态：如果选中，函数返回 `0`，否则返回 `3`。

为了确定缓存应该返回的值（在缓存命中的情况下），Streamlit将复选框的状态（选中/未选中）视为函数`plus_one`的额外输入（就像`x`一样）。如果用户勾选了复选框（从而改变了它的状态），我们会查找缓存以获取相同的`x`值（2）和相同的复选框状态（选中）。如果缓存包含这个输入组合的值，我们就返回它。否则，我们执行该函数并将结果存储在缓存中。

现在让我们了解一下启用和禁用小部件重播如何改变函数的行为。

#### 禁用小部件重播

- 缓存函数中的小部件会抛出`CachedStFunctionWarning`警告，并被忽略。
- 缓存函数中的其他静态元素按预期重播。

#### 启用小部件重播

- 缓存函数中的小部件不会导致警告，并按预期重播。
- 在缓存函数中与小部件进行交互会导致函数再次执行，并更新缓存。
- 在缓存函数中的小部件会在重新运行时保留其状态。
- 每个小部件值的唯一组合被视为函数的单独输入，并用于确定是否应执行该函数。即，每个小部件值的唯一组合都有自己的缓存条目；缓存的函数在第一次运行时执行，并在以后使用保存的值。
- 在一次脚本运行中多次调用具有相同参数的缓存函数会触发`DuplicateWidgetID`错误。
- 如果缓存函数的参数发生变化，重新渲染的小部件会保留其状态。
- 修改缓存函数的源代码会使缓存失效。
- [`st.experimental_memo`](/library/api-reference/performance/st.experimental_memo) 和 [`st.experimental_singleton`](/library/api-reference/performance/st.experimental_singleton) 都支持小部件重放。
- 从根本上讲，当一个函数中包含（支持的）小部件时，无论它是否被`@st.experimental_memo`或`@st.experimental_singleton`装饰，其行为都不会改变。唯一的区别是，只有在检测到缓存"miss"时才会执行该函数。

### 支持的小部件

在被缓存装饰的函数中，支持所有的输入小部件。以下是一个详尽的支持小部件的列表：

- `st.button`
- `st.camera_input`
- `st.checkbox`
- `st.color_picker`
- `st.date_input`
- `st.download_button`
- `st.file_uploader`
- `st.multiselect`
- `st.number_input`
- `st.radio`
- `st.selectbox`
- `st.select_slider`
- `st.slider`
- `st.text_area`
- `st.text_input`
- `st.time_input`