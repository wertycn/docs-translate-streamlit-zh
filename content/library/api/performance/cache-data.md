---
description: st.cache_data is used to cache functions that return data (e.g. dataframe
  transforms, database queries, ML inference).
slug: /library/api-reference/performance/st.cache_data
title: st.cache_data
---

<Tip>

本页面仅包含有关 `st.cache_data` API 的信息。要深入了解缓存及其使用方法，请查看 [Caching](/library/advanced-features/caching)。

</Tip>

<Autofunction function="streamlit.cache_data" />

## 在缓存函数中使用 Streamlit 命令

### 静态元素

从1.16.0版本开始，缓存函数可以包含Streamlit命令！例如，您可以这样做：

```python
@st.cache_data
def get_api_data():
    data = api.get(...)
    st.success("Fetched data from API!")  # 👈 Show a success message
    return data
```

正如我们所知，Streamlit只会在之前没有缓存的情况下运行此函数。在第一次运行时，应用程序会显示`st.success`消息。但是在后续运行中会发生什么？它仍然会显示！Streamlit意识到在缓存的函数内部有一个`st.`命令，并在第一次运行时保存它，并在后续运行中重新播放。重新播放静态元素适用于缓存装饰器。

您还可以使用此功能来缓存整个UI的部分：

```python
@st.cache_data
def show_data():
    st.header("Data analysis")
    data = api.get(...)
    st.success("Fetched data from API!")
    st.write("Here is a plot of the data:")
    st.line_chart(data)
    st.write("And here is the raw data:")
    st.dataframe(data)
```

### 输入小部件

您还可以在缓存函数中使用[交互式输入小部件](/library/api-reference/widgets)，如`st.slider`或`st.text_input`。小部件回放目前是一个实验性功能。要启用它，您需要设置`experimental_allow_widgets`参数：

```python
@st.cache_data(experimental_allow_widgets=True)  # 👈 Set the parameter
def get_data():
    num_rows = st.slider("Number of rows to get")  # 👈 Add a slider
    data = api.get(..., num_rows)
    return data
```

Streamlit将滑块视为缓存函数的额外输入参数。如果您更改滑块位置，Streamlit将检查是否已经对该滑块值进行了缓存。如果是，则返回缓存的值。如果没有，则使用新的滑块值重新运行函数。

在缓存函数中使用小部件非常强大，因为它允许您缓存应用程序的整个部分。但是这也可能带来一定的危险！由于Streamlit将小部件值视为额外的输入参数，它很容易导致过多的内存使用。想象一下，您的缓存函数有五个滑块，并返回一个100MB的DataFrame。然后，对于这五个滑块值的每个排列，都会将100MB添加到缓存中——即使这些滑块并不影响返回的数据！这些添加操作会使您的缓存迅速膨胀。如果您在缓存函数中使用小部件，请注意这个限制。我们建议仅在小部件直接影响缓存返回值的隔离部分中使用此功能。

<警告>

对于缓存函数中的小部件的支持目前处于实验性阶段。我们可能随时更改或删除它，而不会提前警告。请谨慎使用！
</警告>

<注释>

目前在缓存函数中不支持两个小部件：`st.file_uploader`和`st.camera_input`。我们可能在将来支持它们。如果您需要它们，请随时[提交GitHub问题](https://github.com/streamlit/streamlit/issues)！
</注释>