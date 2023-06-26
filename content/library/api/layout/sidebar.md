---
description: st.sidebar displays items in a sidebar.
slug: /library/api-reference/layout/st.sidebar
title: st.sidebar
---

## st.sidebar

## 将小部件添加到侧边栏

您不仅可以使用小部件为应用程序添加交互性，还可以将它们组织到侧边栏中。可以使用对象表示法和`with`表示法将元素传递给`st.sidebar`。

以下两个代码片段是等效的：

```python
# Object notation
st.sidebar.[element_name]
```

```python
# "with" notation
with st.sidebar:
    st.[element_name]
```

每个传递给`st.sidebar`的元素都固定在左侧，使用户能够专注于您应用程序中的内容。

<Tip>

侧边栏是可调整大小的！拖动和放下侧边栏的右边框来调整大小！ ↔️

</Tip>

这是一个示例，演示如何向侧边栏添加选择框和单选按钮：

```python
import streamlit as st

# Using object notation
add_selectbox = st.sidebar.selectbox(
    "How would you like to be contacted?",
    ("Email", "Home phone", "Mobile phone")
)

# Using "with" notation
with st.sidebar:
    add_radio = st.radio(
        "Choose a shipping method",
        ("Standard (5-15 days)", "Express (2-5 days)")
    )
```

<重要>

使用对象表示法不支持的唯一元素是 `st.echo` 和 `st.spinner`。要使用这些元素，您必须使用 `with` 表示法。

以下是如何将 [`st.echo`](/library/api-reference/utilities/st.echo) 和 [`st.spinner`](/library/api-reference/status/st.spinner) 添加到您的侧边栏的示例:

```python
import streamlit as st

with st.sidebar:
    with st.echo():
        st.write("This code will be printed to the sidebar.")

    with st.spinner("Loading..."):
        time.sleep(5)
    st.success("Done!")
```