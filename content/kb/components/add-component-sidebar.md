---
title: 如何将组件添加到侧边栏？
slug: /knowledge-base/components/add-component-sidebar
---

# 如何将组件添加到侧边栏？

您可以使用`with`语法将组件添加到[st.sidebar](/library/api-reference/layout/st.sidebar)中。例如：

```python
with st.sidebar:
    my_component(greeting="hello")
```

实际上，您可以使用`with`语法将组件添加到**任何**[布局容器](/library/api-reference/layout)（例如`st.columns`、`st.expander`）。

```python
col1, col2 = st.columns(2)
with col2:
    my_component(greeting="hello")
```
