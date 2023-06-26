---
slug: /library/advanced-features/connecting-to-data
title: Connecting to data
---

# 连接数据

大多数 Streamlit 应用程序需要某种形式的数据或 API 访问才能发挥作用 - 无论是检索数据以供查看，还是保存某些用户操作的结果。这些数据或 API 通常是某个远程服务，数据库或其他数据源的一部分。

**您可以使用 Python 所能做的任何事情，包括数据连接，在 Streamlit 中通常都能正常工作**。Streamlit 的[教程](/knowledge-base/tutorials/databases)是许多数据源的良好起点。然而：

- 在Python应用程序中连接数据通常是繁琐和令人讨厌的。
- 连接到streamlit应用程序的数据时需要考虑特定的问题，例如缓存和秘钥管理。

**Streamlit提供[`st.experimental_connection()`](/library/api-reference/connections/st.experimental_connection)来更轻松地连接您的Streamlit应用程序与数据和API，只需几行代码即可**。本页面提供了一个基本示例，然后重点介绍高级用法。

要全面了解此功能，请观看Streamlit开发者体验的产品经理Joshua Carroll的视频教程。您将通过使用真实世界的示例，了解在应用程序中创建和管理数据连接的功能。

<YouTube videoId="xQwDfW7UHMo" />

## 基本用法

要获取基本的启动和使用示例，请阅读相关的[数据源教程](/knowledge-base/tutorials/databases)或我们的[博客文章介绍st.experimental_connection](https://blog.streamlit.io/introducing-st-experimental_connection/)。Streamlit内置了与SQL方言和Snowflake Snowpark的连接。我们还提供了可安装的与[云文件存储](https://github.com/streamlit/files-connection)和[Google Sheets](https://github.com/streamlit/gsheets-connection)的连接。

如果您刚开始学习，最好的方式是选择一个可以访问的数据源，并从上面的页面中选择一个最小的示例进行操作。在这里，我们将提供一个超级简单的用法示例，用于使用SQLite数据库。从那里开始，本页面的其余部分将专注于高级用法。

### 一个简单的起点 - 使用本地SQLite数据库

[本地SQLite数据库](https://sqlite.org/index.html)可以用于您的应用程序的半持久性数据存储。

<注意>

社区云应用程序不能保证本地文件存储的持久性，因此平台可以随时删除使用此技术存储的数据。

</Note>

要查看下面的示例实时运行，请查看下面的交互式演示:

<Cloud src="https://st-connection-preview.streamlit.app/SQL?embed=true" />

#### 第一步：安装先决条件库 - SQLAlchemy

Streamlit中的所有SQLConnections都使用SQLAlchemy。对于大多数其他SQL方言，您还需要安装相应的驱动程序。但是，[SQLite驱动程序随python3一起提供](https://docs.python.org/3/library/sqlite3.html)，因此不需要额外安装。

```bash
pip install SQLAlchemy==1.4.0
```

#### 第二步：在Streamlit secrets.toml文件中设置数据库URL

在您的应用程序将要运行的同一目录中创建一个目录和文件`.streamlit/secrets.toml`。将以下内容添加到文件中。

```toml
# .streamlit/secrets.toml

[connections.pets_db]
url = "sqlite:///pets.db"
```

#### 第三步：在您的应用程序中使用连接

以下应用程序创建了一个连接到数据库，使用该连接创建表并插入一些数据，然后查询数据并在数据帧中显示。

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

在这个例子中，我们在对[`conn.query()`](/library/api-reference/connections/st.connections.sqlconnection#sqlconnectionquery)的调用中没有设置`ttl=`值，这意味着只要应用程序服务器运行，Streamlit将无限期地缓存结果。

现在，我们来讨论更高级的主题！🚀

## 高级主题

### 全局秘密、管理多个应用程序和多个数据存储

Streamlit [支持在用户的主目录中指定一个全局的密钥文件](/library/advanced-features/secrets-management)，例如 `~/.streamlit/secrets.toml`。如果您构建或管理多个应用程序，我们建议在本地开发过程中使用一个全局的凭据或密钥文件。通过这种方式，您只需要在一个地方设置和管理凭据，并且将新的应用程序连接到现有的数据源只需要一行代码即可。它还可以减少意外将凭据提交到git中的风险，因为它们不需要存在于项目仓库中。

对于在本地开发期间连接多个类似数据源的情况（例如本地与暂存数据库），您可以在秘钥或凭证文件中为不同环境定义不同的连接部分，然后在运行时决定使用哪个。通过使用 _`name=env:<MY_NAME_VARIABLE>`_ 语法，`st.experimental_connection` 支持这一功能。

例如，假设我有一个本地和一个暂存的MySQL数据库，并且想要在不同的时间将我的应用连接到其中一个。我可以创建一个全局的密钥文件，如下所示：

```toml
# ~/.streamlit/secrets.toml

[connections.local]
url = "mysql://me:****@localhost:3306/local_db"

[connections.staging]
url = "mysql://jdoe:******@staging.acmecorp.com:3306/staging_db"
```

然后，我可以配置我的应用程序连接，使其从指定的环境变量中获取名称。

```python
# streamlit_app.py
import streamlit as st

conn = st.experimental_connection("env:DB_CONN", "sql")
df = conn.query("select * from mytable")
# ...
```

现在，我可以通过设置`DB_CONN`环境变量来在运行时指定是连接到本地还是连接到暂存环境。

```bash
# connect to local
DB_CONN=local streamlit run streamlit_app.py

# connect to staging
DB_CONN=staging streamlit run streamlit_app.py
```

### 高级SQLConnection配置

[SQLConnection](/library/api-reference/connections/st.connections.sqlconnection)的配置使用SQLAlchemy的`create_engine()`函数。它可以接受单个URL参数，或者尝试使用[`SQLAlchemy.engine.URL.create()`](https://docs.sqlalchemy.org/en/20/core/engines.html#sqlalchemy.engine.URL.create)从多个部分（用户名、数据库、主机等）构建URL。

除了URL之外，可以使用`create_engine()`的附加参数来配置一些流行的SQLAlchemy方言，如Snowflake和Google BigQuery。这些参数可以直接通过`st.experimental_connection`调用时的`**kwargs`传递，或者在额外的 secrets 部分中指定为`create_engine_kwargs`。

例如，snowflake-sqlalchemy接受一个额外的`connect_args`参数作为配置的字典，该参数在URL中不支持。可以按照以下方式指定这些参数：[`connect_args`](https://docs.sqlalchemy.org/en/20/core/engines.html#sqlalchemy.create_engine.params.connect_args)

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

另外，这也可以完全在 `**kwargs` 中指定。

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

您还可以同时提供kwargs和secrets.toml的值，它们将被合并（通常，kwargs优先）。

### 在经常使用或长时间运行的应用中的连接考虑事项

默认情况下，连接对象会使用[`st.cache_resource`](/library/api-reference/performance/st.cache_resource)进行缓存，并且没有过期时间。在大多数情况下，这是期望的行为。如果您希望连接对象在一段时间后过期，可以使用`st.experimental_connection('myconn', type=MyConnection, ttl=<N>)`。

许多连接类型预计是长期运行或完全无状态的，因此不需要过期。假设一个连接变得陈旧（例如，缓存的令牌过期或服务器端的连接关闭），那么每个连接都有一个`reset()`方法，它将使缓存版本无效，并导致Streamlit在下次检索时重新创建连接。

通常，像`query()`和`read()`这样的便利方法会使用[`st.cache_data`](/library/api-reference/performance/st.cache_data)来默认缓存结果，不设置过期时间。当应用程序可以执行许多不同的读取操作并且结果较大时，会导致内存使用量过高，并且结果在长时间运行的应用程序中变得陈旧，就像使用`st.cache_data`的任何其他用途一样。对于生产环境的用例，我们建议在这些读取操作上设置适当的`ttl`，例如`conn.read('path/to/file', ttl="1d")`。有关更多信息，请参阅[Caching](/library/advanced-features/caching)。

对于可能有大量并发使用的应用程序，请确保您了解连接的任何线程安全性影响，特别是在使用第三方构建的连接时。由Streamlit构建的连接应默认提供线程安全的操作。

### 构建您自己的连接

使用现有的驱动程序或SDK构建自己的基本连接实现通常是非常简单的。然而，您可以通过进一步的努力添加更复杂的功能。这种自定义实现可以是扩展对新数据源的支持并为Streamlit生态系统做出贡献的一种很好的方式。

对于使用频繁的访问模式和数据源，对许多应用程序进行定制的内部连接实现可以成为组织的一种强大的实践方法。

在下面的st.experimental连接演示应用程序中，查看[构建自己的连接页面](https://experimental-connection.streamlit.app/Build_your_own)以获取快速教程和可工作的实现。此演示在DuckDB之上构建了一个最小但非常实用的连接。

<Cloud src="https://experimental-connection.streamlit.app/Build_your_own?embed=true" />

典型的步骤包括：

1. 声明`Connection`类，继承 [`ExperimentalBaseConnection`](/library/api-reference/connections/st.connections.experimentalbaseconnection) 并将类型参数绑定到底层连接对象:

   ```python
   from streamlit.connections import ExperimentalBaseConnection
   import duckdb

   class DuckDBConnection(ExperimentalBaseConnection[duckdb.DuckDBPyConnection])
   ```

2. 实现`_connect`方法，该方法读取任何kwargs、外部配置/凭据位置和Streamlit secrets来初始化底层连接：

   ```python
   def _connect(self, **kwargs) -> duckdb.DuckDBPyConnection:
       if 'database' in kwargs:
           db = kwargs.pop('database')
       else:
           db = self._secrets['database']
       return duckdb.connect(database=db, **kwargs)
   ```

3. 添加有用的辅助方法，使其与连接相符，并在需要缓存的地方将其包装在`st.cache_data`中。

### 构建连接的最佳实践

我们建议应用以下最佳实践，以使您的连接与Streamlit和更广泛的Streamlit生态系统中的连接保持一致。对于打算公开分发的连接，这些实践尤其重要。

1. **扩展现有的驱动程序或SDK，并默认使用对现有用户有意义的语义。**

   构建连接时，您很少需要从头开始实现复杂的数据访问逻辑。尽可能使用现有的流行Python驱动程序和客户端，这样做可以使您的连接更易于维护、更安全，并且使用户能够使用最新的功能。例如，[SQLConnection](/library/api-reference/connections/st.connections.sqlconnection)扩展了SQLAlchemy，[FileConnection](https://github.com/streamlit/files-connection)扩展了[fsspec](https://filesystem-spec.readthedocs.io/en/latest/)，[GsheetsConnection](https://github.com/streamlit/gsheets-connection)扩展了[gspread](https://docs.gspread.org/en/latest/)，等等。

   考虑使用与底层包一致且对现有用户熟悉的访问模式、方法/参数命名和返回值。

2. **直观、易于使用的读取方法。**

   st.experimental_connection 的强大之处在于提供直观、易于使用的读取方法，使应用开发者能够快速入门。大多数连接应该至少暴露一个直观、易于使用的读取方法，其特点是：

   - 以简单的动词命名，例如`read()`、`query()`或`get()`
   - 默认情况下由`st.cache_data`包装，至少支持`ttl=`参数
   - 如果结果以表格形式返回，则返回一个pandas DataFrame
   - 提供常用的关键字参数（例如分页或格式化）并具有合理的默认值 - 理想情况下，常见情况只需要1-2个参数。

3. **`_connect`方法中的配置、密钥和优先级。**

   每个连接都应该支持通过Streamlit secrets和关键字参数提供的常用连接参数。这些参数的名称应该与初始化或配置底层包时使用的名称相匹配。

   此外，如果适用，连接应通过现有的标准环境变量或配置/凭证文件来支持特定于数据源的配置。在许多情况下，底层包已经提供了构造函数或工厂函数来轻松处理这个问题。

当您可以在多个地方指定相同的连接参数时，我们建议尽可能按照以下优先顺序使用（从高到低）：

- 在代码中指定的关键字参数
   - Streamlit密码
   - 数据源特定的配置（如果适用）

4. **处理线程安全和过时的连接。**

   连接应在可行的情况下提供线程安全操作，并明确记录任何相关注意事项。大多数底层驱动程序或SDK应提供线程安全的对象或方法-尽可能使用这些方法。

   如果底层驱动程序或SDK存在连接对象变得过期或无效的风险，请考虑在访问方法中构建低影响的健康检查或重置/重试模式。Streamlit内置的SQLConnection通过使用[tenacity](https://tenacity.readthedocs.io/)和内置的[Connection.reset()](/library/api-reference/connections/st.connections.sqlconnection#sqlconnectionreset)方法提供了一个很好的示例。另一种方法是鼓励开发人员在`st.experimental_connection()`调用中设置适当的TTL，以确保定期重新初始化连接对象。