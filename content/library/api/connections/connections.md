---
title: 连接和数据库
slug: /library/api-reference/connections
---

# 连接和数据库

## 设置你的连接

<TileContainer>
<RefCard href="/library/api-reference/connections/st.experimental_connection" size="half">

<Image pure alt="screenshot" src="/images/api/connection.jpg" />

#### 创建一个连接

连接到数据源或API

```python
conn = st.experimental_connection('pets_db', type='sql')
pet_owners = conn.query('select * from pet_owners')
st.dataframe(pet_owners)
```

</RefCard>
</TileContainer>

## 内置连接

<TileContainer>

<RefCard href="/library/api-reference/connections/st.connections.sqlconnection" size="half">

<Image pure alt="screenshot" src="/images/api/connections.SQLConnection.jpg" />

#### SQLConnection

使用SQLAlchemy连接到SQL数据库的连接。

```python
conn = st.experimental_connection('sql')
```

</RefCard>

<RefCard href="/library/api-reference/connections/st.connections.snowparkconnection" size="half">

<Image pure alt="screenshot" src="/images/api/connections.SnowparkConnection.jpg" />

#### SnowparkConnection

连接到Snowflake Snowpark的连接。

```python
conn = st.experimental_connection('snowpark')
```

</RefCard>
</TileContainer>

## 第三方连接

<TileContainer>
<RefCard href="/library/api-reference/connections/st.connections.experimentalbaseconnection" size="half">

#### 连接基类

使用 `ExperimentalBaseConnection` 构建自己的连接。

```python
class MyConnection(ExperimentalBaseConnection[myconn.MyConnection]):
    def _connect(self, **kwargs) -> MyConnection:
        return myconn.connect(**self._secrets, **kwargs)
    def query(self, query):
        return self._instance.query(query)
```

</RefCard>

</TileContainer>
