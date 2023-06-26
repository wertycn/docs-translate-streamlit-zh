---
标题：st.experimental_memo
网址：/library/api-reference/performance/st.experimental_memo
描述：st.experimental_memo用于缓存函数的执行结果。

<Important>
这是一个实验性功能。实验性功能及其API可能随时更改或删除。要了解更多信息，请点击[这里](/library/advanced-features/prerelease#experimental-features)。
</Important>

<Autofunction function="streamlit.experimental_memo" deprecated={true} deprecatedText="<code>st.experimental_memo</code>在1.18.0版本中已被弃用。请使用<a href='/library/api-reference/performance/st.cache_data'><code>st.cache_data</code></a>代替。在<a href='/library/advanced-features/caching'>缓存</a>中了解更多信息。"/>

持久性记忆缓存当前不支持TTL。如果指定了`persist`，则`ttl`将被忽略：

```python
import streamlit as st

@st.experimental_memo(ttl=60, persist="disk")
def load_data():
    return 42

st.write(load_data())
```

并且会在终端中记录一个警告：

```bash
streamlit run app.py

  You can now view your Streamlit app in your browser.
  Local URL: http://localhost:8501
  Network URL: http://192.168.1.1:8501

2022-09-22 13:35:41.587 The memoized function 'load_data' has a TTL that will be ignored. Persistent memo caches currently don't support TTL.
```

### 在缓存装饰的函数中回放静态 `st` 元素

使用 `@st.experimental_memo` 装饰的函数可以包含静态 `st` 元素。当执行缓存装饰的函数时，我们记录生成的元素和阻塞的消息，因此即使由于结果被缓存而跳过函数的执行，这些元素也会出现在应用程序中。

在下面的示例中，`@st.experimental_memo` 装饰器用于缓存 `load_data` 函数的执行结果，该函数返回一个 pandas DataFrame。请注意，缓存的函数还包含一个 `st.area_chart` 命令，当函数被跳过时，该命令将被重放，因为结果已经被缓存。

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

`cache-decorated` 函数中支持的静态 `st` 元素包括:

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
- `st.experimental_set_query_params` - 设置查询参数
- `st.form` - 表单
- `st.form_submit_button` - 表单提交按钮
- `st.graphviz_chart` - 绘制Graphviz图表
- `st.help` - 显示帮助信息
- `st.image` - 显示图像
- `st.info` - 显示信息
- `st.json` - 显示JSON数据
- `st.latex` - 显示LaTeX数学公式
- `st.line_chart` - 绘制折线图
- `st.markdown` - 显示Markdown文本
- `st.metric` - 显示指标
- `st.plotly_chart` - 绘制Plotly图表
- `st.progress` - 显示进度条
- `st.pydeck_chart` - 绘制PyDeck图表
- `st.snow` - 显示雪花效果
- `st.spinner` - 显示加载动画
- `st.success` - 显示成功信息
- `st.table` - 显示表格
- `st.text` - 显示文本
- `st.vega_lite_chart` - 绘制Vega-Lite图表
- `st.video` - 显示视频
- `st.warning` - 显示警告信息

### 在使用缓存装饰函数时重新播放输入小部件

除了静态元素外，使用`@st.experimental_memo`装饰的函数还可以包含[input widgets](/library/api-reference/widgets)！回放输入小部件默认是禁用的。要启用它，您可以将`@st.experimental_memo`的`experimental_allow_widgets`参数设置为`True`。下面的示例启用了小部件回放，并展示了在缓存装饰的函数中使用复选框小部件的示例。

```python
import streamlit as st

# Enable widget replay
@st.experimental_memo(experimental_allow_widgets=True)
def func():
    # Contains an input widget
    st.checkbox("Works!")

func()
```

如果使用缓存装饰的函数包含输入小部件，但是`experimental_allow_widgets`设置为`False`或未设置，则Streamlit将抛出`CachedStFunctionWarning`警告，如下所示：

```python
import streamlit as st

# Widget replay is disabled by default
@st.experimental_memo
def func():
    # Streamlit will throw a CachedStFunctionWarning
    st.checkbox("Doesn't work")

func()
```

![cached-st-function-warning-widgets](/images/cached-st-function-warning-widgets.png)

### 小部件回放的工作原理

让我们揭开缓存装饰函数中小部件回放的神秘面纱，并获得概念上的理解。小部件值被视为函数的额外输入，并用于确定是否应该执行函数。考虑以下示例：

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

`plus_one` 函数接受一个整数 `x` 作为输入，并返回 `x + 1`。该函数还包含一个复选框小部件，用于"清零" `x` 的值。即 `plus_one` 的返回值取决于复选框的状态：如果复选框被选中，则函数返回 `0`，否则返回 `3`。

为了确定缓存应该返回哪个值（在缓存命中的情况下），Streamlit将复选框的状态（选中/未选中）视为函数`plus_one`的另一个输入（就像`x`一样）。如果用户勾选复选框（从而改变其状态），我们会查找具有相同`x`值（2）和相同复选框状态（选中）的缓存。如果缓存中包含这个输入组合的值，我们就返回它。否则，我们执行函数并将结果存储在缓存中。

现在让我们了解一下启用和禁用小部件回放如何改变函数的行为。

#### 禁用小部件回放

- 缓存函数中的小部件会引发`CachedStFunctionWarning`警告，并被忽略。
- 缓存函数中的其他静态元素会按预期重新播放。

#### 启用小部件回放

- 缓存函数中的小部件不会引发警告，并按预期重新播放。
- 在缓存函数中与小部件进行交互会导致函数重新执行，并更新缓存。
- 缓存函数中的小部件在重新运行时保留其状态。
- 每个不同的小部件值组合被视为函数的独立输入，并用于确定是否应该执行该函数。即每个不同的小部件值组合都有自己的缓存条目；缓存的函数在第一次运行时执行，并在之后使用保存的值。
- 在一个脚本运行中多次调用具有相同参数的缓存函数会触发 `DuplicateWidgetID` 错误。
- 如果缓存函数的参数发生变化，再次渲染的小部件将保留其状态。
- 更改缓存函数的源代码会使缓存失效。
- [`st.experimental_memo`](/library/api-reference/performance/st.experimental_memo) 和 [`st.experimental_singleton`](/library/api-reference/performance/st.experimental_singleton) 都支持小部件重放。
- 从根本上讲，当一个带有（支持的）小部件的函数被装饰为`@st.experimental_memo`或`@st.experimental_singleton`时，其行为不会发生变化。唯一的区别是当我们检测到缓存“未命中”时，函数才会被执行。

### 支持的小部件

在使用缓存装饰的函数中，所有的输入小部件都被支持。以下是支持的小部件的详尽列表:

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
