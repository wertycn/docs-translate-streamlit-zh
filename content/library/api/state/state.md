---
title: 会话状态
slug: /library/api-reference/session-state
description: st.session_state 是一种在每个用户会话中共享变量的方法。
---

# 会话状态

会话状态是一种在每个用户会话中共享变量的方法。除了存储和持久化状态的能力外，Streamlit还提供了使用回调函数来操作状态的能力。会话状态还可以在[multipage app](/library/get-started/multipage-apps)的应用程序之间进行持久化。

请查看Streamlit开发者倡导者Dr. Marisa Smith的Session State基础教程视频，以便开始：

<YouTube videoId="92jUAXBmZyU" />

### 在Session State中初始化值

Session State API采用基于字段的API，与Python字典非常相似：

```python
# Initialization
if 'key' not in st.session_state:
    st.session_state['key'] = 'value'

# Session State also supports attribute based syntax
if 'key' not in st.session_state:
    st.session_state.key = 'value'
```

### 读取和更新

通过将其传递给 `st.write`，读取会话状态中项目的值并将其显示出来:

```python
# Read
st.write(st.session_state.key)

# Outputs: value
```

通过给会话状态的某个项赋值来更新它。

```python
st.session_state.key = 'value2'     # Attribute API
st.session_state['key'] = 'value2'  # Dictionary like API
```

对于会话状态(Session State)中的内容感到好奇吗？使用 `st.write` 或者魔术命令(magic)来查看。

```python
st.write(st.session_state)

# With magic:
st.session_state
```

如果访问了一个未初始化的变量，Streamlit会抛出一个方便的异常。

```python
st.write(st.session_state['value'])

# Throws an exception!
```

![state-uninitialized-exception](/images/state_uninitialized_exception.png)

### 删除项目

使用删除任何Python字典中项目的语法来删除会话状态中的项目：

```python
# Delete a single key-value pair
del st.session_state[key]

# Delete all the items in Session state
for key in st.session_state.keys():
    del st.session_state[key]
```

会话状态也可以通过转到“设置”→“清除缓存”，然后重新运行应用程序来清除。

![state-clear-cache](/images/clear_cache.png)

### 会话状态和小部件状态关联

每个具有键的小部件会自动添加到会话状态中：

```python
st.text_input("Your name", key="name")

# This exists now:
st.session_state.name
```

### 使用回调函数更新会话状态

回调函数是一个Python函数，在输入小部件发生变化时被调用。

**执行顺序**：在响应**事件**更新会话状态时，回调函数先执行，然后从上到下执行应用程序。

可以使用`on_change`（或`on_click`）、`args`和`kwargs`参数与小部件一起使用回调函数：

**参数**

- **on_change** 或 **on_click** - 用作回调函数的函数名称
- **args**（_tuple_）- 要传递给回调函数的参数列表
- **kwargs**（_dict_）- 要传递给回调函数的命名参数

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

要添加一个回调函数，请在小部件声明之前定义一个回调函数，并通过`on_change`（或`on_click`）参数将其传递给小部件。

### 表单和回调

表单中的小部件可以通过会话状态API访问和设置其值。`st.form_submit_button`可以与回调函数关联。在单击提交按钮时，会执行回调函数。例如：

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

- 只有 `st.form_submit_button` 在表单中具有回调功能。其他表单内的小部件不允许具有回调功能。
- `on_change` 和 `on_click` 事件只支持输入类型的小部件。
- 在通过 Session state API 实例化小部件后，不允许通过修改小部件的值，否则将引发 `StreamlitAPIException`。例如：

  ```python
  slider = st.slider(
      label='我的滑块', min_value=1,
      max_value=10, value=5, key='my_slider')

  st.session_state.my_slider = 7

  # 抛出异常!
  ```

  ![state-modified-instantiated-exception](/images/state_modified_instantiated_exception.png)

- 不建议通过会话状态API设置小部件状态并在小部件声明中使用`value`参数，这将在首次运行时抛出警告。例如:

  ```python
  st.session_state.my_slider = 7

  slider = st.slider(
      label='选择一个值'，min_value=1，max_value=10，value=5，key='my_slider')

  ```

  ![state-value-api-exception](/images/state_value_api_exception.png)

- 不允许通过会话状态 API 来设置 `st.button`、`st.download_button` 和 `st.file_uploader` 这类按钮样式小部件的状态。这类小部件的默认状态为 _False_，具有短暂的 _True_ 状态，只在单次运行中有效。例如：

  ```python
  if 'my_button' not in st.session_state:
      st.session_state.my_button = True

  st.button('My button', key='my_button')

  # Throws an exception!
  ```

  ![state-button-exception](/images/state_button_exception.png)
