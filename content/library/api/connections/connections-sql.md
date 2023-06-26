---
slug: /library/api-reference/connections/st.connections.sqlconnection
title: st.connections.SQLConnection
---

<重要>

这是一个实验性功能。实验性功能及其API可能随时更改或删除。要了解更多信息，请点击[此处](/library/advanced-features/prerelease#experimental-features)。

</重要>

<提示>

此页面仅包含`st.connections.SQLConnection` API的内容。要深入了解在Streamlit应用中创建和管理数据连接，请阅读[连接到数据](/library/advanced-features/connecting-to-data)。

</提示>

<Autofunction function="streamlit.connections.SQLConnection" />

### 基本用法：

```python
import streamlit as st

conn = st.experimental_connection("sql")
df = conn.query("select * from pet_owners")
st.dataframe(df)
```

如果您想直接传递连接URL（或其他参数），也是可以的:

```python
conn = st.experimental_connection(
    "local_db",
    type="sql",
    url="mysql://user:pass@localhost:3306/mydb"
)
```

或在 [secrets](/library/advanced-features/secrets-management) 中指定参数：

```toml
# .streamlit/secrets.toml
[connections.mydb]
dialect = "mysql"
username = "myuser"
password = "password"
host = "localhost"
database = "mydb"
```

```python
# streamlit_app.py
conn = st.experimental_connection("mydb", type="sql", autocommit=True)
```

正如上述所述，一些云数据库使用额外的`**kwargs`来指定凭据。可以通过使用`create_engine_kwargs`部分来传递这些凭据。

```toml
# .streamlit/secrets.toml
[connections.snowflake]
url = "snowflake://<username>@<account>/"

[connections.snowflake.create_engine_kwargs.connect_args]
authenticator = "externalbrowser"
role = "..."
# ...
```

<Autofunction function="streamlit.connections.SQLConnection.query" />

<Autofunction function="streamlit.connections.SQLConnection.reset" />

<Autofunction function="streamlit.connections.SQLConnection.session" />