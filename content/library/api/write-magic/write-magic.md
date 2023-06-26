---
slug: /library/api-reference/write-magic
title: st.write and magic commands
---

# st.write和魔术命令

Streamlit有两种简单的方式将信息显示在您的应用程序中，这通常是您尝试的第一件事：`st.write`和魔术命令。

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

#### Magic

Any time Streamlit sees either a variable or literal value on its own line, it automatically writes that to your app using `st.write`

```python
"Hello **world**!"
my_data_frame
my_mpl_figure
```

</RefCard>
</TileContainer>