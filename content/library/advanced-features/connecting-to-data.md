---
title: 连接到数据
slug: /library/advanced-features/connecting-to-data
---

# 连接到数据

大多数 Streamlit 应用程序需要某种形式的数据或 API 访问才能发挥作用 - 无论是检索数据以供查看还是保存某些用户操作的结果。这些数据或 API 通常是远程服务、数据库或其他数据源的一部分。

**使用Python可以做的任何事情，包括数据连接，在Streamlit中通常都可以工作**。Streamlit的[教程](/knowledge-base/tutorials/databases)是许多数据源的绝佳起点。但是：

- 在Python应用程序中连接数据通常是乏味和烦人的。
- 从Streamlit应用程序连接数据有特定的考虑因素，如缓存和密钥管理。

**Streamlit提供了[`st.experimental_connection()`](/library/api-reference/connections/st.experimental_connection)函数，可以通过几行代码更轻松地将您的Streamlit应用程序连接到数据和API**。本页面提供了一个基本示例，然后重点介绍了高级用法。

要全面了解此功能，请观看由Joshua Carroll（Streamlit的开发者体验产品经理）制作的视频教程。您将通过使用真实示例，了解该功能在创建和管理应用程序中的数据连接方面的实用性。

<YouTube videoId="xQwDfW7UHMo" />

## 基本用法

对于基本的启动和使用示例，请阅读相关的 [数据源教程](/knowledge-base/tutorials/databases) 或我们的 [博客文章介绍 st.experimental_connection](https://blog.streamlit.io/introducing-st-experimental_connection/)。Streamlit 内置了与 SQL 方言和 Snowflake Snowpark 的连接。我们还维护了可安装的连接 [Cloud 文件存储](https://github.com/streamlit/files-connection) 和 [Google Sheets](https://github.com/streamlit/gsheets-connection)。

如果您刚开始学习，最好的方法是选择一个可以访问的数据源，并从上面的页面中获取一个最小的示例进行操作。在这里，我们将提供一个使用SQLite数据库的超小使用示例。接下来，本页的其余部分将重点介绍高级用法。

### 简单的起点 - 使用本地SQLite数据库

一个[本地SQLite数据库](https://sqlite.org/index.html)可能对您的应用程序的半持久化数据存储非常有用。

<Note>

社区云应用程序不能保证本地文件存储的持久性，因此平台可能随时删除使用此技术存储的数据。

</Note>

要查看下面的示例实时运行，请查看下面的交互式演示：

<Cloud src="https://st-connection-preview.streamlit.app/SQL?embed=true" />

#### 第一步：安装先决条件库 - SQLAlchemy

在Streamlit中，所有的SQL连接都使用SQLAlchemy。对于大多数其他SQL方言，您还需要安装相应的驱动程序。但是，[SQLite驱动程序随python3一起安装](https://docs.python.org/3/library/sqlite3.html)，所以不是必需的。

```bash
pip install SQLAlchemy==1.4.0
```

#### 步骤2：在Streamlit的secrets.toml文件中设置数据库URL

在与您的应用程序运行的相同目录中创建一个名为`.streamlit/secrets.toml`的目录和文件。将以下内容添加到文件中。

```toml
# .streamlit/secrets.toml

[connections.pets_db]
url = "sqlite:///pets.db"
```

#### 第三步：在您的应用程序中使用连接

以下的应用程序创建了一个连接到数据库，使用该连接创建了一个表并插入了一些数据，然后查询数据并将其显示在一个数据框中。

```python
# streamlit_app.py

import streamlit as st

# Create the SQL connection to pets_db as specified in your secrets file.
conn = st.experimental_connection('pets_db', type='sql')

# Insert some data with conn.session.
with conn.session as s:
    s.execute('CREATE TABLE IF NOT EXISTS pet_owners (person TEXT, pet TEXT);')
    s.execute('DELETE FROM pet_owners;')
    pet_owners = {'jerry': 'fish', 'barbara': 'cat', 'alex': 'puppy'}
    for k in pet_owners:
        s.execute(
            'INSERT INTO pet_owners (person, pet) VALUES (:owner, :pet);',
            params=dict(owner=k, pet=pet_owners[k])
        )
    s.commit()

# Query and display the data you inserted
pet_owners = conn.query('select * from pet_owners')
st.dataframe(pet_owners)
```

在这个例子中，我们在调用[`conn.query()`](/library/api-reference/connections/st.connections.sqlconnection#sqlconnectionquery)时没有设置`ttl=`值，这意味着只要应用服务器运行，Streamlit会无限期地缓存结果。

现在，让我们来看一些更高级的话题！🚀

## 高级话题

### 全局密钥、管理多个应用程序和多个数据存储

Streamlit [支持在用户的主目录中指定的全局 secrets 文件](/library/advanced-features/secrets-management)，例如 `~/.streamlit/secrets.toml`。如果您构建或管理多个应用程序，我们建议在本地开发过程中使用全局凭证或秘密文件。通过这种方式，您只需在一个地方设置和管理您的凭证，在现有数据源上连接新应用程序只需要一行代码即可实现。它还降低了意外将凭证提交到 git 的风险，因为它们不需要存在于项目存储库中。

对于在本地开发过程中连接多个类似数据源的情况（例如本地和暂存数据库），您可以在秘钥或凭证文件中为不同的环境定义不同的连接部分，然后在运行时决定使用哪个。`st.experimental_connection` 支持使用 _`name=env:<MY_NAME_VARIABLE>`_ 语法来实现这一点。

例如，假设我有一个本地和一个暂存的MySQL数据库，并且想要在不同的时间将我的应用程序连接到其中一个。我可以创建一个全局的秘密文件，如下所示：

```toml
# ~/.streamlit/secrets.toml

[connections.local]
url = "mysql://me:****@localhost:3306/local_db"

[connections.staging]
url = "mysql://jdoe:******@staging.acmecorp.com:3306/staging_db"
```

然后，我可以配置我的应用连接，从指定的环境变量中获取其名称。

```python
# streamlit_app.py
import streamlit as st

conn = st.experimental_connection("env:DB_CONN", "sql")
df = conn.query("select * from mytable")
# ...
```

现在我可以通过设置`DB_CONN`环境变量来指定在运行时连接到本地还是暂存环境。

```bash
# connect to local
DB_CONN=local streamlit run streamlit_app.py

# connect to staging
DB_CONN=staging streamlit run streamlit_app.py
```

### 高级SQLConnection配置

[SQLConnection](/library/api-reference/connections/st.connections.sqlconnection) 的配置使用SQLAlchemy的 `create_engine()` 函数。该函数接受一个URL参数或尝试使用多个部分（用户名、数据库、主机等）构建一个URL，使用 [`SQLAlchemy.engine.URL.create()`](https://docs.sqlalchemy.org/en/20/core/engines.html#sqlalchemy.engine.URL.create)。

除了URL之外，还可以使用`create_engine()`的其他参数来配置一些流行的SQLAlchemy方言，比如Snowflake和Google BigQuery。这些参数可以直接作为`**kwargs`传递给[st.experimental_connection](/library/api-reference/connections/st.experimental_connection)调用，或者在额外的`create_engine_kwargs`部分中指定。

例如，snowflake-sqlalchemy接受一个额外的[`connect_args`](https://docs.sqlalchemy.org/en/20/core/engines.html#sqlalchemy.create_engine.params.connect_args)参数作为配置的字典，该参数在URL中不支持。可以按以下方式指定这些参数：

```toml
# .streamlit/secrets.toml

[connections.snowflake]
url = "snowflake://<user_login_name>@<account_identifier>/"

[connections.snowflake.create_engine_kwargs.connect_args]
authenticator = "externalbrowser"
warehouse = "xxx"
role = "xxx"
```

```python
# streamlit_app.py

import streamlit as st

# url and connect_args from secrets.toml above are picked up and used here
conn = st.experimental_connection("snowflake", "sql")
# ...
```

或者，这也可以完全在 `**kwargs` 中指定。

```python
# streamlit_app.py

import streamlit as st

# secrets.toml is not needed
conn = st.experimental_connection(
    "snowflake",
    "sql",
    url = "snowflake://<user_login_name>@<account_identifier>/",
    connect_args = dict(
        authenticator = "externalbrowser",
        warehouse = "xxx",
        role = "xxx",
    )
)
# ...
```

您还可以提供kwargs和secrets.toml的值，它们会被合并（通常，kwargs具有优先权）。

### 在经常使用或长时间运行的应用中的连接考虑事项

默认情况下，使用[`st.cache_resource`](/library/api-reference/performance/st.cache_resource)缓存连接对象，且不会过期。在大多数情况下，这是期望的。如果您希望连接对象在一段时间后过期，可以使用`st.experimental_connection('myconn', type=MyConnection, ttl=<N>)`。

许多连接类型预计是长期运行或完全无状态的，因此不需要过期。假设连接变得陈旧（例如缓存的令牌过期或服务器端连接关闭）。在这种情况下，每个连接都有一个`reset()`方法，它将使缓存的版本失效，并导致Streamlit在下次检索时重新创建连接。

通常，像`query()`和`read()`这样的便捷方法在默认情况下会使用[`st.cache_data`](/library/api-reference/performance/st.cache_data)进行结果的缓存，而且没有过期时间。当应用程序可以运行许多不同的读取操作，并且结果很大时，会导致内存使用量增加，并且结果在长时间运行的应用程序中变得过时，就像使用`st.cache_data`的任何其他用法一样。对于生产环境的用例，我们建议在这些读取操作上设置适当的`ttl`，例如`conn.read('path/to/file', ttl="1d")`。有关更多信息，请参阅[Caching](/library/advanced-features/caching)。

对于可能具有大量并发使用的应用程序，请确保了解连接的任何线程安全性影响，特别是在使用由第三方构建的连接时。由Streamlit构建的连接应默认提供线程安全的操作。

### 构建自己的连接

使用现有的驱动程序或SDK构建自己的基本连接实现通常非常简单。但是，您可以通过进一步的努力添加更复杂的功能。此自定义实现可以是扩展对新数据源的支持并为Streamlit生态系统做出贡献的好方法。

对于频繁使用的访问模式和数据源，跨多个应用程序维护一个定制的内部连接实现可以是组织的一种有力实践。

请在下面的st.experimental连接演示应用程序中查看[构建自己的连接页面](https://experimental-connection.streamlit.app/Build_your_own)，以获取快速教程和可工作的实现。此演示在DuckDB之上构建了一个简约但非常实用的连接。

<Cloud src="https://experimental-connection.streamlit.app/Build_your_own?embed=true" />

典型的步骤包括：

1. 声明 Connection 类，继承 [`ExperimentalBaseConnection`](/library/api-reference/connections/st.connections.experimentalbaseconnection)，并将类型参数绑定到底层连接对象：

   ```python
   from streamlit.connections import ExperimentalBaseConnection
   import duckdb

   class DuckDBConnection(ExperimentalBaseConnection[duckdb.DuckDBPyConnection])
   ```

2. 实现`_connect`方法，该方法读取任何kwargs、外部配置/凭据位置和Streamlit secrets，以初始化底层连接：

   ```python
   def _connect(self, **kwargs) -> duckdb.DuckDBPyConnection:
       if 'database' in kwargs:
           db = kwargs.pop('database')
       else:
           db = self._secrets['database']
       return duckdb.connect(database=db, **kwargs)
   ```

3. 添加有用的辅助方法，以便与连接器保持一致（在需要缓存的地方使用 `st.cache_data` 进行包装）

### 构建连接器的最佳实践

我们建议采用以下最佳实践，使您的连接器与 Streamlit 和更广泛的 Streamlit 生态系统中内置的连接器保持一致。对于您打算公开分发的连接器，这些实践尤为重要。

1. **扩展现有的驱动程序或SDK，并默认为现有用户提供有意义的语义。**

   在构建连接时，您很少需要从头开始实现复杂的数据访问逻辑。尽可能使用现有的流行Python驱动程序和客户端。这样做可以更容易地维护您的连接，更安全，并使用户能够获得最新的功能。例如，[SQLConnection](/library/api-reference/connections/st.connections.sqlconnection)扩展了SQLAlchemy，[FileConnection](https://github.com/streamlit/files-connection)扩展了[fsspec](https://filesystem-spec.readthedocs.io/en/latest/)，[GsheetsConnection](https://github.com/streamlit/gsheets-connection)扩展了[gspread](https://docs.gspread.org/en/latest/)等。

   考虑使用与底层软件包一致且对现有用户熟悉的访问模式、方法/参数命名和返回值。

2. **直观、易于使用的读取方法。**

   st.experimental_connection 的强大之处在于提供直观、易于使用的读取方法，帮助应用开发人员快速入门。大多数连接应该至少暴露一个直观、易于使用的读取方法，它应该具备以下特点：

   - 以简单的动词命名，例如 `read()`、`query()` 或 `get()`
   - 默认情况下使用 `st.cache_data` 进行包装，至少支持 `ttl=` 参数
   - 如果结果以表格格式呈现，则返回一个 pandas DataFrame
   - 提供常用的关键字参数（如分页或格式化），并具有合理的默认值 - 理想情况下，常见情况只需要 1-2 个参数。

3. **在 `_connect` 方法中配置、密码和优先级。**

   每个连接都应支持通过Streamlit secrets和关键字参数提供的常用连接参数。这些名称应与初始化或配置底层包时使用的名称匹配。

   此外，在相关的情况下，连接应通过现有的标准环境变量或配置/凭证文件支持数据源特定的配置。在许多情况下，底层包提供了构造函数或工厂函数，可以轻松处理这些配置。

当您可以在多个位置指定相同的连接参数时，我们建议按照以下优先级顺序进行设置（从高到低）：

- 在代码中指定的关键字参数
   - Streamlit机密信息
  - 数据源特定配置（如果适用）

4. **处理线程安全性和过期连接。**

   当可行时，连接应提供线程安全的操作，并清楚地记录任何与此相关的注意事项。大多数底层驱动程序或SDK应提供线程安全的对象或方法-在可能的情况下使用这些方法。

   如果底层驱动程序或SDK存在状态连接对象变得过期或无效的风险，请考虑在访问方法中构建低影响的健康检查或重置/重试模式。Streamlit内置的SQLConnection使用[tenacity](https://tenacity.readthedocs.io/)和内置的[Connection.reset()](/library/api-reference/connections/st.connections.sqlconnection#sqlconnectionreset)方法提供了一个很好的示例。另一种方法是鼓励开发人员在`st.experimental_connection()`调用中设置适当的TTL，以确保定期重新初始化连接对象。
