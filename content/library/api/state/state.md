---
description: st.session_state is a way to share variables between reruns, for each
  user session.
slug: /library/api-reference/session-state
title: Session State
---

# 会话状态

会话状态是一种在每个用户会话中共享变量的方式。除了存储和持久化状态的能力外，Streamlit还提供了使用回调函数来操作状态的能力。会话状态还可以在多页面应用程序中跨应用程序持久化。

通过观看Streamlit开发者倡导者Dr. Marisa Smith的这个会话状态基础教程视频，开始学习吧：

<YouTube videoId="92jUAXBmZyU" />

### 在会话状态中初始化值

会话状态API采用基于字段的API，与Python字典非常相似:

```python
# Initialization
if 'key' not in st.session_state:
    st.session_state['key'] = 'value'

# Session State also supports attribute based syntax
if 'key' not in st.session_state:
    st.session_state.key = 'value'
```

### 读取和更新

通过将值传递给 `st.write` ，读取会话状态中的项目的值并显示出来：

```python
# Read
st.write(st.session_state.key)

# Outputs: value
```

通过为其分配一个值来更新会话状态中的项目：

```python
st.session_state.key = 'value2'     # Attribute API
st.session_state['key'] = 'value2'  # Dictionary like API
```

想知道Session State中有什么内容吗？使用`st.write`或magic命令：

```python
st.write(st.session_state)

# With magic:
st.session_state
```

当访问一个未初始化的变量时，Streamlit会抛出一个有用的异常。

```python
st.write(st.session_state['value'])

# Throws an exception!
```

![state-uninitialized-exception](/images/state_uninitialized_exception.png)

### 删除项目

使用删除任何Python字典中的项目的语法来删除会话状态中的项目：

```python
# Delete a single key-value pair
del st.session_state[key]

# Delete all the items in Session state
for key in st.session_state.keys():
    del st.session_state[key]
```

您可以通过转到“设置”→“清除缓存”，然后重新运行应用程序来清除会话状态。

![state-clear-cache](/images/clear_cache.png)

### 会话状态和小部件状态的关联

每个带有键的小部件都会自动添加到会话状态中。

```python
st.text_input("Your name", key="name")

# This exists now:
st.session_state.name
```

### 使用回调函数更新会话状态

回调函数是一个Python函数，在输入小部件发生变化时被调用。

**执行顺序**：在响应**事件**更新会话状态时，回调函数首先被执行，然后从上到下执行应用程序。

可以使用参数`on_change`（或`on_click`）、`args`和`kwargs`与小部件一起使用回调函数：

**参数**

- **on_change** 或 **on_click** - 用作回调函数的函数名
- **args**（_元组_）- 要传递给回调函数的参数列表
- **kwargs**（_字典_）- 要传递给回调函数的命名参数

支持`on_change`事件的小部件：

- `st.checkbox`
- `st.color_picker`
- `st.date_input`
- `st.multiselect`
- `st.number_input`
- `st.radio`
- `st.select_slider`
- `st.selectbox`
- `st.slider`
- `st.text_area`
- `st.text_input`
- `st.time_input`
- `st.file_uploader`

支持`on_click`事件的小部件：

- `st.button`
- `st.download_button`
- `st.form_submit_button`

要添加一个回调函数，需要在小部件声明之前定义一个回调函数，并通过 `on_change`（或 `on_click` ）参数将其传递给小部件。

### 表单和回调

在表单中的小部件的值可以通过会话状态API进行访问和设置。`st.form_submit_button` 可以关联一个回调函数。当点击提交按钮时，回调函数将被执行。例如：

```python
def form_callback():
    st.write(st.session_state.my_slider)
    st.write(st.session_state.my_checkbox)

with st.form(key='my_form'):
    slider_input = st.slider('My slider', 0, 10, 5, key='my_slider')
    checkbox_input = st.checkbox('Yes or No', key='my_checkbox')
    submit_button = st.form_submit_button(label='Submit', on_click=form_callback)
```

### 注意事项和限制

- 只有`st.form_submit_button`在表单中有回调功能。表单中的其他小部件不允许有回调。
- `on_change`和`on_click`事件仅支持输入类型的小部件。
- 在实例化后通过Session状态API修改小部件的值是不允许的，这将引发`StreamlitAPIException`异常。例如：

  ```python
  slider = st.slider(
      label='我的滑块', min_value=1,
      max_value=10, value=5, key='my_slider')

  st.session_state.my_slider = 7

  # 抛出异常!
  ```

  ![state-modified-instantiated-exception](/images/state_modified_instantiated_exception.png)

- 不推荐使用会话状态API设置小部件状态并在小部件声明中使用`value`参数，这将在第一次运行时引发警告。例如:

  ```python
  st.session_state.my_slider = 7

  slider = st.slider(
      label='选择一个值'，min_value=1，
      max_value=10，value=5，key='my_slider')

  ```

  ![state-value-api-exception](/images/state_value_api_exception.png)

- 不允许通过会话状态API设置类似按钮的小部件的状态：`st.button`，`st.download_button`和`st.file_uploader`。这些类型的小部件默认情况下为_False_，并且具有临时的_True_状态，仅在单个运行中有效。例如：

  ```python
  if 'my_button' not in st.session_state:
      st.session_state.my_button = True

  st.button('我的按钮', key='my_button')

  # 抛出异常!
  ```

  ![state-button-exception](/images/state_button_exception.png)