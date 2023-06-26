---
title: st.write和魔术命令
slug: /library/api-reference/write-magic
---

# st.write和魔术命令

Streamlit有两种简单的方法来将信息显示到您的应用程序中，这通常是您尝试的第一件事：`st.write`和魔术命令。

<TileContainer>
<RefCard href="/library/api-reference/write-magic/st.write">

#### st.write

将参数写入应用程序。

```python
st.write("Hello **world**!")
st.write(my_data_frame)
st.write(my_mpl_figure)
```

</RefCard>
<RefCard href="/library/api-reference/write-magic/magic">

#### 魔法

每当Streamlit在自己的一行上看到一个变量或字面值时，它会自动使用`st.write`将其写入您的应用程序。

```python
"Hello **world**!"
my_data_frame
my_mpl_figure
```

</RefCard>
</TileContainer>
