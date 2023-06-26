---
slug: /knowledge-base/using-streamlit/batch-elements-input-widgets-form
title: Batch elements and input widgets with st.form
---

# 使用 st.form 批量处理元素和输入小部件

让我们来看一下如何使用 [`st.form`](/library/api-reference/control-flow/st.form) 来批量处理元素和输入小部件。

在 Streamlit 中，每个小部件的交互都会导致应用程序的重新运行。然而，有时候您可能希望与多个小部件进行交互，并在触发应用程序的单次重新运行时提交这些交互。

使用 `st.form`，您可以将输入小部件批量处理，以及其他的元素。
`st.form_submit_button` 通过单击一个按钮提交这些小部件中的状态。

```python
# Forms can be declared using the 'with' syntax
with st.form(key='my_form'):
    text_input = st.text_input(label='Enter your name')
    submit_button = st.form_submit_button(label='Submit')
```

```python
# Alternative syntax, declare a form and use the returned object
form = st.form(key='my_form')
form.text_input(label='Enter some text')
submit_button = form.form_submit_button(label='Submit')
```

```python
# st.form_submit_button returns True upon form submit
if submit_button:
    st.write(f'hello {name}')
```

表单可以出现在应用程序的任何位置（侧边栏、列等），但有一些**限制条件**：

- 表单中的小部件不能相互依赖，即 `widget1` 的_输出_不能作为表单中 `widget2` 的_输入_。
- 根据设计，与 `st.form` 内的小部件交互不会触发重新运行。因此，`st.button` 不能在 `st.form` 中声明。
- `st.form` 不能嵌套在另一个 `st.form` 中。
- 表单必须有一个关联的 `st.form_submit_button`。点击该按钮会触发重新运行。如果表单没有关联的 `st.form_submit_button`，Streamlit会抛出错误。