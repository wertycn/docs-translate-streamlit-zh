---
slug: /knowledge-base/tutorials/databases/snowflake
title: Connect Streamlit to Snowflake
---

# 将Streamlit连接到Snowflake

## 介绍

本指南将解释如何通过Streamlit安全地访问Snowflake数据库。它使用[st.experimental_connection](/library/api-reference/connections/st.experimental_connection)、[Snowpark Python](https://docs.snowflake.com/en/developer-guide/snowpark/python/index)库以及Streamlit的[secrets management](/library/advanced-features/secrets-management)。下面的示例代码**仅适用于Streamlit版本>= 1.22**，在此版本中添加了`st.experimental_connection`功能。

跳转到底部了解有关[使用Snowflake Connector for Python进行连接的信息](#使用Snowflake-Connector-for-Python)。

## 创建一个Snowflake数据库

<注意>

如果您已经有一个要使用的数据库，可以随意[跳过下一步](#将用户名和密码添加到您的本地应用程序密钥)。

</注意>

首先，[注册Snowflake](https://signup.snowflake.com/)并登录[Snowflake Web界面](https://docs.snowflake.com/en/user-guide/connecting.html#logging-in-using-the-web-interface)（记下您的用户名、密码和[账户标识符](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html)）：

![](/images/databases/snowflake-1.png)

在Worksheets页面的SQL编辑器中输入以下查询，创建一个数据库和一个包含一些示例值的表格：

```sql
CREATE DATABASE PETS;

CREATE TABLE MYTABLE (
    NAME            varchar(80),
    PET             varchar(80)
);

INSERT INTO MYTABLE VALUES ('Mary', 'dog'), ('John', 'cat'), ('Robert', 'bird');

SELECT * FROM MYTABLE;
```

在执行查询之前，首先确定您正在使用的Snowflake UI / Web界面。下面的示例使用[Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight)。您也可以使用[Classic Console Worksheets](https://docs.snowflake.com/en/user-guide/ui-worksheet)或其他任何运行Snowflake SQL语句的方式。

### 在工作表中执行查询

要在工作表中执行查询，请使用鼠标突出显示或选择所有查询，并单击右上角的播放按钮。

![AWS截图1](/images/databases/snowflake-4.png)

<重要>

在点击播放按钮之前，请确保突出显示或选择**所有**查询（1-10行）。

</重要>

执行查询后，您应该在页面底部的**结果**面板中看到表的预览。此外，您还应该通过展开页面左侧的手风琴菜单来查看您新创建的数据库和模式。最后，仓库名称显示在**共享**按钮左侧的按钮上。

请确保记下您的仓库、数据库和模式的名称。☝️

![AWS截图2](/images/databases/snowflake-5.png)

## 安装 snowflake-snowpark-python

您可以在 [Snowpark 开发人员指南](https://docs.snowflake.com/en/developer-guide/snowpark/python/setup) 中找到安装 `snowflake-snowpark-python` 的说明和先决条件。

```bash
pip install "snowflake-snowpark-python[pandas]"
```

需要特别强调的先决条件:

- 目前只支持Python 3.8。
- 确保您已经安装了适用于`snowflake-snowpark-python`版本的正确的pyarrow版本。如果有疑问，在安装snowflake-snowpark-python之前尝试卸载pyarrow。

## 在本地应用程序密钥中添加连接参数

您本地的Streamlit应用程序将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取机密信息。如果该文件尚不存在，请创建并按照下面的示例添加您的Snowflake用户名、密码、[账户标识符](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html)，以及您的仓库、数据库和模式名称。有关[Streamlit机密管理的更多信息，请点击这里](/library/advanced-features/secrets-management)。

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

如果您在前一步创建了数据库，则数据库和模式的名称分别为`PETS`和`PUBLIC`。如果可用，Streamlit还将使用[SnowSQL配置文件](https://docs.snowflake.com/en/user-guide/snowsql-config#snowsql-config-file)中的**Snowflake配置和凭证**。

<重要提示>

将此文件添加到`.gitignore`中，并不要将其提交到您的GitHub存储库中！

</重要提示>

## 编写您的Streamlit应用程序

将以下代码复制到您的Streamlit应用程序中并运行。确保调整查询以使用您的表的名称。

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

请查看上面的 `st.experimental_connection`。它处理了密钥的检索、设置、查询缓存和重试。默认情况下，`query()` 的结果会被缓存而不会过期。在这种情况下，我们设置 `ttl=600` 来确保查询结果的缓存时间不超过10分钟。您还可以设置 `ttl=0` 来禁用缓存。了解更多信息请参阅 [Caching](/library/advanced-features/caching)。

如果一切顺利（且您使用了我们上面创建的示例表格），您的应用程序应该看起来像这样：

![应用程序截图](/images/databases/snowflake-app.png)

### 使用Snowpark会话

与上面使用的[SnowparkConnection](/library/api-reference/connections/st.connections.snowparkconnection)相同，还提供了对[Snowpark会话](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/session.html)的访问，用于在Snowflake内部本地运行的DataFrame风格操作。使用这种方法，您可以按以下方式重写上面的应用程序：

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

这个示例使用 `with conn.safe_session()` 来提供线程安全性。`conn.session` 也可以直接使用，但不保证线程安全。如果一切顺利（并且您使用了我们上面创建的示例表），您的应用程序应该与上面第一个示例的屏幕截图相同。

## 使用Snowflake Connector for Python

在某些情况下，您可能更喜欢使用[Snowflake Connector for Python](https://docs.snowflake.com/en/developer-guide/python-connector/python-connector)而不是Snowpark Python。Streamlit通过[SQLConnection](/library/api-reference/connections/st.connections.sqlconnection)和[snowflake-sqlalchemy](https://docs.snowflake.com/en/developer-guide/python-connector/sqlalchemy)库原生支持这一点。

```bash
pip install snowflake-sqlalchemy
```

安装`snowflake-sqlalchemy`将同时安装所有必要的依赖项。

配置凭据遵循`SQLConnection`格式，稍有不同。有关更多详细信息，请参阅Snowflake SQLAlchemy [配置参数](https://docs.snowflake.com/en/developer-guide/python-connector/sqlalchemy#connection-parameters)文档。

```toml
# .streamlit/secrets.toml

[connections.snowflake]
url = "snowflake://<user_login_name>:<password>@<account_identifier>/<database_name>/<schema_name>?warehouse=<warehouse_name>&role=<role_name>"
```

或者，可以像下面所示那样使用`authenticator`或密钥对身份验证来指定连接参数，使用`create_engine_kwargs`。

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

在您的应用程序中初始化和使用连接是相似的。请注意，[SQLConnection.query()](/library/api-reference/connections/st.connections.sqlconnection#sqlconnectionquery) 支持额外的参数，如 `params` 和 `chunksize`，这对于更高级的应用程序可能会很有用。

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

如果一切顺利（并且您使用了我们在上面创建的示例表格），您的应用程序应该与上面第一个示例中的截图相同。

## 从Community Cloud连接到Snowflake

本教程假设您使用的是本地的Streamlit应用程序，但您也可以从托管在Community Cloud中的应用程序连接到Snowflake。主要的附加步骤包括：

- 使用`requirements.txt`文件包含关于依赖项的信息，包括`snowflake-snowpark-python`和其他依赖项。
- 在您的Community Cloud应用程序中[添加您的秘密信息](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management#deploy-an-app-and-set-up-secrets)。
- 对于使用`snowflake-snowpark-python`的应用程序，您还应确保应用程序在[Python 3.8上运行](/streamlit-community-cloud/get-started/deploy-an-app#advanced-settings-for-deployment)。