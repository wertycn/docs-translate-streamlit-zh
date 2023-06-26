---
title: st.sidebar
slug: /library/api-reference/layout/st.sidebar
description: st.sidebar用于在侧边栏显示项目。
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

每个传递给`st.sidebar`的元素都会固定在左侧，让用户专注于应用程序中的内容。

<提示>

侧边栏是可调整大小的！拖动和放下侧边栏的右边框来调整大小！↔️

</提示>

下面是一个示例，展示如何在侧边栏中添加一个选择框和一个单选按钮：

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

在对象表示法中不支持的唯一元素是`st.echo`和`st.spinner`。要使用这些元素，您必须使用`with`表示法。

以下是如何将[`st.echo`](/library/api-reference/utilities/st.echo)和[`st.spinner`](/library/api-reference/status/st.spinner)添加到侧边栏的示例：

```python
import streamlit as st

with st.sidebar:
    with st.echo():
        st.write("This code will be printed to the sidebar.")

    with st.spinner("Loading..."):
        time.sleep(5)
    st.success("Done!")
```
