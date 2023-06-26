---
slug: /library/api-reference/control-flow
title: Control flow
---

# 控制流

## 更改执行流程

默认情况下，Streamlit应用程序会完全执行脚本，但是我们允许一些功能来处理应用程序中的控制流程。

<TileContainer>
<RefCard href="/library/api-reference/control-flow/st.stop">

#### 停止执行

立即停止执行。

```python
st.stop()
```

</RefCard>

<RefCard href="/library/api-reference/control-flow/st.experimental_rerun">

#### 重新运行脚本

立即重新运行脚本。

```python
st.experimental_rerun()
```

</RefCard>
</TileContainer>

## 组合多个小部件

默认情况下，Streamlit在用户与应用程序进行交互时会重新运行脚本。
然而，有时候在实际重新运行脚本之前，等待一组相关的小部件被填充，会提供更好的用户体验。这就是`st.form`的用途！

<TileContainer>
<RefCard href="/library/api-reference/control-flow/st.form">

#### 表单

创建一个批量元素与"提交"按钮一起的表单。

```python
with st.form(key="my_form"):
    username = st.text_input("Username")
    password = st.text_input("Password")
    st.form_submit_button("Login")
```

</RefCard>

<RefCard href="/library/api-reference/control-flow/st.form_submit_button">

#### 表单提交按钮

显示一个表单提交按钮。

```python
with st.form(key="my_form"):
    username = st.text_input("Username")
    password = st.text_input("Password")
    st.form_submit_button("Login")
```

</RefCard>

</TileContainer>

<ComponentSlider>

<ComponentCard href="https://github.com/kmcgrady/streamlit-autorefresh">

<Image pure alt="screenshot" src="/images/api/components/autorefresh.jpg" />

#### 自动刷新

在不阻塞脚本的情况下强制刷新。由[@kmcgrady](https://github.com/kmcgrady)创建。

```python
from streamlit_autorefresh import st_autorefresh

st_autorefresh(interval=2000, limit=100,
  key="fizzbuzzcounter")
```

</ComponentCard>

<ComponentCard href="https://github.com/lukasmasuch/streamlit-pydantic">

<Image pure alt="screenshot" src="/images/api/components/pydantic.jpg" />

#### Pydantic

从Pydantic模型和Dataclasses自动生成Streamlit用户界面。由[@lukasmasuch](https://github.com/lukasmasuch)创建。

```python
import streamlit_pydantic as sp

sp.pydantic_form(key="my_form",
  model=ExampleModel)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/st_pages">

<Image pure alt="screenshot" src="/images/api/components/pages.jpg" />

#### Streamlit Pages

Streamlit多页面应用的实验版本。由[@blackary](https://github.com/blackary)创建。

```python
from st_pages import Page, show_pages, add_page_title

show_pages([ Page("streamlit_app.py", "Home", "🏠"),
  Page("other_pages/page2.py", "Page 2", ":books:"), ])
```

</ComponentCard>

</ComponentSlider>