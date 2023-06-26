---
title: 使用 st.form 进行批量元素和输入小部件
slug: /knowledge-base/using-streamlit/batch-elements-input-widgets-form
---

# 使用 st.form 进行批量元素和输入小部件

让我们来看看如何使用 [`st.form`](/library/api-reference/control-flow/st.form) 来批量处理元素和输入小部件。

在 Streamlit 中，每次小部件交互都会导致应用程序重新运行。然而，
有时您可能希望与一些小部件进行交互，并且
通过触发一次应用程序的单次重新运行，提交这些交互。

使用 `st.form`，您可以将批量输入小部件与 `st.form_submit_button` 一起组合，并通过单击一个按钮提交这些小部件中的状态。

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

表单可以出现在应用程序的任何位置（侧边栏、列等），但有一些**约束条件**：

- 表单中的小部件不能相互依赖，即`widget1`的_output_不能作为`widget2`的_input_。
- 根据设计，与`st.form`中的小部件交互不会触发重新运行。因此，不能在`st.form`内声明`st.button`。
- 不能在另一个`st.form`中嵌入`st.form`。
- 表单必须有一个关联的`st.form_submit_button`。点击此按钮会触发重新运行。如果一个表单没有关联的`st.form_submit_button`，Streamlit会抛出一个错误。
