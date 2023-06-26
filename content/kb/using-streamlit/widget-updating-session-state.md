---
slug: /knowledge-base/using-streamlit/widget-updating-session-state
title: Widget updating for every second input when using session state
---

# 使用会话状态时，每秒更新小部件的输入

## 概述

您正在使用 [session state](/library/api-reference/session-state) 来存储应用程序中的页面交互。当用户与应用程序中的小部件交互时（例如，点击按钮），您希望应用程序更新其小部件状态并反映新的值。然而，您注意到这并没有发生。相反，用户必须两次与小部件进行交互（例如，点击按钮两次）才能使应用程序显示正确的值。现在你该怎么办呢？🤔让我们在下面的部分中详细介绍解决方案。

## 解决方案

当使用会话状态来更新脚本中的小部件或值时，您需要使用您分配给小部件的唯一键，而不是您分配给小部件的变量。在下面的示例代码块中，分配给滑块小部件的唯一**键**是`slider`，而分配给小部件的**变量**是`slide_val`。

让我们通过一个示例来说明。假设您希望用户点击一个按钮来重置一个滑块。

要在按钮点击时更新滑块的值，您需要使用一个[回调函数](/library/api-reference/session-state#use-callbacks-to-update-session-state)，并将其与[`st.button`](/library/api-reference/widgets/st.button)的`on_click`参数一起使用：

```python
# the callback function for the button will add 1 to the
# slider value up to 10
def plus_one():
    if st.session_state["slider"] < 10:
        st.session_state.slider += 1
    else:
        pass
    return

# when creating the button, assign the name of your callback
# function to the on_click parameter
add_one = st.button("Add one to the slider", on_click=plus_one, key="add_one")

# create the slider
slide_val = st.slider("Pick a number", 0, 10, key="slider")
```

## 相关资源

- [缓存 Sqlite DB 连接导致页面渲染出现故障](https://discuss.streamlit.io/t/caching-sqlite-db-connection-resulting-in-glitchy-rendering-of-the-page/19017)
- [与选项框链接的全选复选框](https://discuss.streamlit.io/t/select-all-checkbox-that-is-linked-to-selectbox-of-options/18521)