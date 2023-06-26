---
title: 将Streamlit连接到Snowflake
slug: /knowledge-base/tutorials/databases/snowflake
---

# 将Streamlit连接到Snowflake

## 简介

本指南介绍了如何从Streamlit安全地访问Snowflake数据库。它使用[st.experimental_connection](/library/api-reference/connections/st.experimental_connection)、[Snowpark Python](https://docs.snowflake.com/en/developer-guide/snowpark/python/index)库和Streamlit的[secrets管理](/library/advanced-features/secrets-management)。下面的示例代码**仅适用于Streamlit版本>= 1.22**，该版本引入了`st.experimental_connection`。

跳转到底部了解关于[使用Snowflake Connector for Python连接](#using-the-snowflake-connector-for-python)的信息。

## 创建一个Snowflake数据库

<Note>

如果您已经有一个要使用的数据库，可以随意[跳过下一步](#add-username-and-password-to-your-local-app-secrets)。

</Note>

首先，[注册Snowflake](https://signup.snowflake.com/)并登录[Snowflake Web界面](https://docs.snowflake.com/en/user-guide/connecting.html#logging-in-using-the-web-interface)（记录下您的用户名、密码和[账户标识符](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html)）：

![](/images/databases/snowflake-1.png)

在“Worksheets”页面的SQL编辑器中输入以下查询来创建一个数据库和一个包含一些示例值的表格：

```sql
CREATE DATABASE PETS;

CREATE TABLE MYTABLE (
    NAME            varchar(80),
    PET             varchar(80)
);

INSERT INTO MYTABLE VALUES ('Mary', 'dog'), ('John', 'cat'), ('Robert', 'bird');

SELECT * FROM MYTABLE;
```

在执行查询之前，请先确定您正在使用的Snowflake UI/ Web界面。下面的示例使用[Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight)。您还可以使用[经典控制台工作表](https://docs.snowflake.com/en/user-guide/ui-worksheet)或其他方式来运行Snowflake SQL语句。

### 在工作表中执行查询

要在工作表中执行查询，请使用鼠标突出显示或选择所有查询，并单击右上角的播放按钮。

![AWS屏幕截图1](/images/databases/snowflake-4.png)

<重要>

在单击播放按钮之前，请确保突出显示或选择**所有**查询（第1至10行）。

</重要>

执行完查询后，您应该可以在页面底部的**结果**面板中看到表的预览。此外，展开页面左侧的手风琴菜单，您还应该可以看到新创建的数据库和模式。最后，仓库的名称显示在**分享**按钮左侧的按钮上。

<Image alt="AWS screenshot 2" src="/images/databases/snowflake-5.png" />

请确保记下您的仓库、数据库和模式的名称。 ☝️

## 安装 snowflake-snowpark-python

您可以在[Snowpark开发者指南](https://docs.snowflake.com/en/developer-guide/snowpark/python/setup)中找到安装`snowflake-snowpark-python`的指导和先决条件。

```bash
pip install "snowflake-snowpark-python[pandas]"
```

需要特别注意的先决条件：

- 目前只支持Python 3.8。
- 确保您已安装了适用于您的`snowflake-snowpark-python`版本的正确的pyarrow版本。如果有疑问，请在安装snowflake-snowpark-python之前尝试卸载pyarrow。

## 将连接参数添加到本地应用程序机密中

您的本地Streamlit应用程序将从位于应用程序根目录中的`.streamlit/secrets.toml`文件中读取密钥。如果该文件不存在，请创建它，并按照下面的示例添加您的Snowflake用户名、密码、[帐户标识符](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html)以及您的仓库、数据库和架构名称。有关[Streamlit密钥管理的更多信息，请点击此处](/library/advanced-features/secrets-management)。

```toml
# .streamlit/secrets.toml

[connections.snowpark]
account = "xxx"
user = "xxx"
password = "xxx"
role = "xxx"
warehouse = "xxx"
database = "xxx"
schema = "xxx"
client_session_keep_alive = true
```

如果您已经按照之前的步骤创建了数据库，那么您的数据库名称和架构名称分别为 `PETS` 和 `PUBLIC`。如果可用，Streamlit 还将使用 [SnowSQL 配置文件](https://docs.snowflake.com/en/user-guide/snowsql-config#snowsql-config-file) 中的 **Snowflake 配置和凭据**。

<重要提示>

将此文件添加到 `.gitignore` 中，并不要将其提交到您的 GitHub 仓库中！

</重要提示>

## 编写您的 Streamlit 应用程序

将以下代码复制到您的Streamlit应用程序并运行。确保根据您的表格名称调整查询。

```python
# streamlit_app.py

import streamlit as st

# Initialize connection.
conn = st.experimental_connection('snowpark')

# Perform query.
df = conn.query('SELECT * from mytable;', ttl=600)

# Print results.
for row in df.itertuples():
    st.write(f"{row.NAME} has a :{row.PET}:")
```

请参考上面的 `st.experimental_connection`？它处理了密钥的检索、设置、查询缓存和重试。默认情况下，`query()` 的结果会被缓存而不过期。在这种情况下，我们设置了 `ttl=600`，以确保查询结果最多被缓存10分钟。您也可以设置 `ttl=0` 来禁用缓存。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例表），您的应用程序应该如下所示：

![应用程序截图](/images/databases/snowflake-app.png)

### 使用Snowpark会话

与上述相同的[SnowparkConnection](/library/api-reference/connections/st.connections.snowparkconnection)也提供了对[Snowpark会话](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/session.html)的访问，用于在Snowflake内部本地运行DataFrame风格的操作。使用这种方法，您可以将上述应用程序重写如下：

```python
# streamlit_app.py

import streamlit as st

# Initialize connection.
conn = st.experimental_connection('snowpark')

# Load the table as a dataframe using the Snowpark Session.
@st.cache_data
def load_table():
    with conn.safe_session() as session:
        return session.table('mytable').to_pandas()

df = load_table()

# Print results.
for row in df.itertuples():
    st.write(f"{row.NAME} has a :{row.PET}:")
```

这个示例使用`with conn.safe_session()`来提供线程安全性。`conn.session`也可以直接使用，但不保证线程安全。如果一切正常（并且您使用了我们在上面创建的示例表），您的应用程序应该与上面第一个示例中的截图一样。

## 使用Snowflake Connector for Python

在某些情况下，您可能更喜欢使用[Snowflake Connector for Python](https://docs.snowflake.com/en/developer-guide/python-connector/python-connector) 而不是Snowpark Python。Streamlit通过[SQLConnection](/library/api-reference/connections/st.connections.sqlconnection)和[snowflake-sqlalchemy](https://docs.snowflake.com/en/developer-guide/python-connector/sqlalchemy)库来支持这一点。

```bash
pip install snowflake-sqlalchemy
```

安装 `snowflake-sqlalchemy` 还会安装所有必要的依赖项。

配置凭据遵循 `SQLConnection` 格式，略有不同。有关更多详细信息，请参阅Snowflake SQLAlchemy [配置参数](https://docs.snowflake.com/en/developer-guide/python-connector/sqlalchemy#connection-parameters) 文档。

```toml
# .streamlit/secrets.toml

[connections.snowflake]
url = "snowflake://<user_login_name>:<password>@<account_identifier>/<database_name>/<schema_name>?warehouse=<warehouse_name>&role=<role_name>"
```

或者，您可以像下面所示一样使用`authenticator`或密钥对身份验证来指定连接参数，使用`create_engine_kwargs`。

```toml
# .streamlit/secrets.toml

[connections.snowflake]
url = "snowflake://<user_login_name>@<account_identifier>/"

[connections.snowflake.create_engine_kwargs.connect_args]
authenticator = "externalbrowser"
warehouse = "xxx"
role = "xxx"
client_session_keep_alive = true
```

在您的应用程序中初始化和使用连接是类似的。请注意，[SQLConnection.query()](/library/api-reference/connections/st.connections.sqlconnection#sqlconnectionquery) 支持额外的参数，比如 `params` 和 `chunksize`，这在更高级的应用程序中可能会很有用。

```python
# streamlit_app.py

import streamlit as st

# Initialize connection.
conn = st.experimental_connection('snowflake', type='sql')

# Perform query.
df = conn.query('SELECT * from mytable;', ttl=600)

# Print results.
for row in df.itertuples():
    st.write(f"{row.name} has a :{row.pet}:")
```

如果一切顺利（并且您使用了我们在上面创建的示例表格），您的应用程序应该与上面第一个示例的截图相同。

## 从Community Cloud连接到Snowflake

本教程假设您使用的是本地的Streamlit应用程序，但您也可以从托管在Community Cloud中的应用程序连接到Snowflake。主要的额外步骤包括：

- [使用 `requirements.txt` 文件包含依赖信息](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies)，其中包括 `snowflake-snowpark-python` 和其他依赖项。
- [将您的机密信息添加到您的 Community Cloud 应用程序中](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management#deploy-an-app-and-set-up-secrets)。
- 对于使用 `snowflake-snowpark-python` 的应用程序，您还需要确保该应用程序在 [python 3.8 上运行](/streamlit-community-cloud/get-started/deploy-an-app#advanced-settings-for-deployment)。
