---
slug: /library/api-reference/utilities
title: Placeholders, help, and options
---

# 占位符、帮助和选项

有几种方法可以在您的应用程序中创建占位符，使用文档字符串提供帮助，获取和修改配置选项和查询参数。

<TileContainer>
<RefCard href="/library/api-reference/utilities/st.set_page_config">

#### 设置页面标题、网站图标等

配置页面的默认设置。

```python
st.set_page_config(
  title="My app",
  favicon=":shark:",
)
```

</RefCard>
<RefCard href="/library/api-reference/utilities/st.echo">

<!--<Image pure alt="screenshot" src="/images/api/echo.jpg" />-->

#### Echo

在应用程序上显示一些代码，然后执行它。对于教程非常有用。

```python
with st.echo():
  st.write('This code will be printed')
```

</RefCard>
<RefCard href="/library/api-reference/utilities/st.help">

#### 获取帮助

显示对象的文档字符串，以漂亮的格式呈现。

```python
st.help(st.write)
st.help(pd.DataFrame)
```

</RefCard>

<RefCard href="/library/api-reference/utilities/st.experimental_get_query_params">

#### 获取查询参数

返回当前显示在浏览器URL栏中的查询参数。

```python
st.experimental_get_query_params()
```

</RefCard>

<RefCard href="/library/api-reference/utilities/st.experimental_set_query_params">

#### 设置查询参数

设置在浏览器的URL栏中显示的查询参数。

```python
st.experimental_set_query_params(
  show_map=True,
  selected=["asia"]
)
```

</RefCard>
</TileContainer>