---
title: 将 Streamlit 连接到 Microsoft SQL Server
slug: /knowledge-base/tutorials/databases/mssql
---

# 将 Streamlit 连接到 Microsoft SQL Server

## 简介

本指南将说明如何从 Streamlit Community Cloud 安全地访问 **_远程_** 的 Microsoft SQL Server 数据库。它使用 [pyodbc](https://github.com/mkleehammer/pyodbc/wiki) 库和 Streamlit 的 [secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management) 功能。

## 创建一个SQL Server数据库

<注>

如果您已经有一个远程数据库想要使用，可以随意[跳到下一步](#add-username-and-password-to-your-local-app-secrets)。

</注>

首先，请按照Microsoft的文档安装[SQL Server](https://docs.microsoft.com/en-gb/sql/sql-server/?view=sql-server-ver15)和`sqlcmd` [实用程序](https://docs.microsoft.com/en-gb/sql/tools/sqlcmd-utility?view=sql-server-ver15)。它们提供了详细的安装指南，包括如何：

- [在Windows上安装SQL Server](https://docs.microsoft.com/en-gb/sql/database-engine/install-windows/install-sql-server?view=sql-server-ver15)
- [在 Red Hat Enterprise Linux 上安装](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-red-hat?view=sql-server-ver15)
- [在 SUSE Linux Enterprise Server 上安装](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-suse?view=sql-server-ver15)
- [在 Ubuntu 上安装](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver15)
- [在 Docker 上运行](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15)
- [在 Azure 中配置 SQL 虚拟机](https://docs.microsoft.com/zh-cn/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

安装 SQL Server 后，请在设置过程中记录下 SQL Server 的名称、用户名和密码。

## 本地连接

如果您正在本地连接，请使用`sqlcmd`命令连接到您的本地SQL Server实例。

1. 在终端中运行以下命令:

   ```bash
   sqlcmd -S localhost -U SA -P '<YourPassword>'
   ```

   由于您是在本地连接，所以SQL Server名称为`localhost`，用户名为`SA`，密码是您在SA账户设置过程中提供的密码。

2. 如果成功，您应该看到一个**sqlcmd**命令提示符`1>`。

3. 如果遇到连接失败的情况，请查看 Microsoft 针对您的操作系统的连接故障排除建议（[Linux](https://docs.microsoft.com/en-gb/sql/linux/sql-server-linux-troubleshooting-guide?view=sql-server-ver15#connection) 和 [Windows](https://docs.microsoft.com/en-gb/sql/linux/sql-server-linux-troubleshooting-guide?view=sql-server-ver15#connection)）。

<Tip>

当远程连接时，SQL Server名称为机器名称或IP地址。您可能还需要在防火墙上打开SQL Server的TCP端口（默认为1433）。

</Tip>

### 创建SQL Server数据库

现在，您已经运行了SQL Server并使用`sqlcmd`连接到它！ 🥳 让我们通过创建一个包含一些示例值的表的数据库来利用它。

1. 从`sqlcmd`命令提示符下，运行以下Transact-SQL命令来创建一个名为`mydb`的测试数据库：

   ```sql
   创建数据库 `mydb`

```
2. 要执行上述命令，请在新行上输入 `GO` :

```
sql
GO
```

### 插入一些数据

接下来，在 `mydb` 数据库中创建一个名为 `mytable` 的新表，该表具有三列和两行。

1. 切换到新的 `mydb` 数据库：

```sql
USE mydb
```

2. 使用以下模式创建一个新表：

```sql
CREATE TABLE mytable (name varchar(80), pet varchar(80))
```

3. 在表中插入一些数据：

```sql
   将用户名和密码添加到本地应用程序秘钥

您的本地Streamlit应用程序将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取密钥。如果该文件不存在，请创建该文件，并按照以下示例添加SQL Server名称、数据库名称、用户名和密码：

```toml
# .streamlit/secrets.toml

server = "localhost"
database = "mydb"
username = "SA"
password = "xxx"
```

<重要>

在将应用程序秘钥复制到Streamlit Community Cloud时，请确保用您的远程SQL Server的**server**、**database**、**username**和**password**值替换它们！

并将此文件添加到`.gitignore`中，不要将其提交到您的GitHub存储库。

</重要>

## 将应用程序秘钥复制到Streamlit Community Cloud

由于上面的`secrets.toml`文件未提交到GitHub，您需要单独将其内容传递给部署在Streamlit社区云上的应用程序。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑 Secrets**。将`secrets.toml`的内容复制到文本区域中。更多信息请参阅[Secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将pyodbc添加到您的requirements文件

为了使用Streamlit在本地连接到SQL Server，除了在SQL Server安装过程中安装的Microsoft ODBC驱动程序之外，您还需要`pip install pyodbc`。

在_Streamlit Cloud_上，我们内置了对SQL Server的支持。根据广大用户的需求，我们直接将SQL Server工具包括ODBC驱动程序和可执行文件`sqlcmd`和`bcp`添加到了容器镜像中，以供Cloud应用使用，因此您无需安装它们。

您只需要将[`pyodbc`](https://github.com/mkleehammer/pyodbc) Python包添加到您的`requirements.txt`文件中，就可以开始使用了！🎈

```bash
# requirements.txt
pyodbc==x.x.x
```

将 `x.x.x` ☝️ 替换为您想要在云中安装的 pyodbc 版本。

<注意>

目前，Streamlit Community Cloud 不支持 Azure Active Directory 身份验证。我们将在支持 Azure Active Directory 时更新本教程。

</注意>

## 编写您的 Streamlit 应用程序

将下面的代码复制到您的 Streamlit 应用程序中并运行。确保适应 `query` 使用您的表的名称。

```python
import streamlit as st
import pyodbc

# Initialize connection.
# Uses st.cache_resource to only run once.
@st.cache_resource
def init_connection():
    return pyodbc.connect(
        "DRIVER={ODBC Driver 17 for SQL Server};SERVER="
        + st.secrets["server"]
        + ";DATABASE="
        + st.secrets["database"]
        + ";UID="
        + st.secrets["username"]
        + ";PWD="
        + st.secrets["password"]
    )

conn = init_connection()

# Perform query.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def run_query(query):
    with conn.cursor() as cur:
        cur.execute(query)
        return cur.fetchall()

rows = run_query("SELECT * from mytable;")

# Print results.
for row in rows:
    st.write(f"{row[0]} has a :{row[1]}:")

```

请参考上面的`st.cache_data`吗？如果没有它，Streamlit将在每次应用程序重新运行时运行查询（例如在小部件交互时）。使用`st.cache_data`，它只有在查询更改或者在10分钟后才运行（这就是`ttl`的作用）。请注意：如果您的数据库更新频率更高，您应该适应`ttl`或者移除缓存，以便观看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了上面创建的示例表格），您的应用程序应该如下图所示：

![应用程序截图](/images/databases/streamlit-app.png)
