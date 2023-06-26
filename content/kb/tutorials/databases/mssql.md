---
slug: /knowledge-base/tutorials/databases/mssql
title: Connect Streamlit to Microsoft SQL Server
---

# 将 Streamlit 连接到 Microsoft SQL Server

## 简介

本指南说明如何从 Streamlit Community Cloud 安全地访问一个**_远程_**的 Microsoft SQL Server 数据库。它使用 [pyodbc](https://github.com/mkleehammer/pyodbc/wiki) 库和 Streamlit 的 [secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建一个 SQL Server 数据库

<Note>

如果您已经有一个远程数据库需要使用，可以自由地跳到下一步（添加用户名和密码到本地应用程序机密）。

</Note>

首先，按照微软文档的指引安装 [SQL Server](https://docs.microsoft.com/en-gb/sql/sql-server/?view=sql-server-ver15) 和 `sqlcmd` [实用工具](https://docs.microsoft.com/en-gb/sql/tools/sqlcmd-utility?view=sql-server-ver15)。它们提供了详细的安装指南，包括如何：

- [在Windows上安装SQL Server](https://docs.microsoft.com/zh-cn/sql/database-engine/install-windows/install-sql-server?view=sql-server-ver15)
- [在Red Hat Enterprise Linux上安装](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-red-hat?view=sql-server-ver15)
- [在SUSE Linux Enterprise Server上安装](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-suse?view=sql-server-ver15)
- [在Ubuntu上安装](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver15)
- [在Docker上运行](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15)
- [在Azure中预配SQL虚拟机](https://docs.microsoft.com/zh-cn/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

一旦您安装了SQL Server，请在设置过程中记录下SQL Server的名称、用户名和密码。

## 本地连接

如果您正在本地连接，请使用`sqlcmd`连接到您的本地SQL Server实例。

1. 在终端中运行以下命令：

   ```bash
   sqlcmd -S localhost -U SA -P '<YourPassword>'
   ```

   由于您是在本地连接，SQL Server的名称为`localhost`，用户名为`SA`，密码是您在SA账户设置过程中提供的密码。

2. 如果成功，您应该看到一个 **sqlcmd** 命令提示符 `1>`。

3. 如果遇到连接失败的情况，请查看微软针对您的操作系统提供的连接故障排除建议（[Linux](https://docs.microsoft.com/zh-cn/sql/linux/sql-server-linux-troubleshooting-guide?view=sql-server-ver15#connection) 和 [Windows](https://docs.microsoft.com/zh-cn/sql/linux/sql-server-linux-troubleshooting-guide?view=sql-server-ver15#connection)）。

<Tip>

在远程连接时，SQL Server名称是机器名称或IP地址。您可能还需要在防火墙上打开SQL Server TCP端口（默认为1433）。

### 创建一个SQL Server数据库

现在，您已经运行了SQL Server并使用`sqlcmd`连接到它！ 🥳 让我们通过创建一个包含一些示例值的表的数据库来使用它。

1. 从`sqlcmd`命令提示符处运行以下Transact-SQL命令来创建一个名为`mydb`的测试数据库：

   ```sql
   创建数据库 mydb
   ```

2. 要执行上述命令，请在新行上键入 `GO` ：

   ```sql
   GO
   ```

### 插入一些数据

接下来，在 `mydb` 数据库中创建一个名为 `mytable` 的新表，该表有三列和两行数据。

1. 切换到新的 `mydb` 数据库：

   ```sql
   USE mydb
   ```

2. 使用以下架构创建一个新表：

   ```sql
   CREATE TABLE mytable (name varchar(80), pet varchar(80))
   ```

3. 向表中插入一些数据：

   ```sql
   将用户名和密码添加到本地应用程序密钥中

您的本地Streamlit应用将从应用根目录中的`.streamlit/secrets.toml`文件中读取机密信息。如果该文件尚不存在，请创建该文件，并按照下面所示添加SQL Server名称、数据库名称、用户名和密码：

```toml
# .streamlit/secrets.toml

server = "localhost"
database = "mydb"
username = "SA"
password = "xxx"
```

<重要>

在将应用程序密钥复制到Streamlit社区云时，请确保将**server**、**database**、**username**和**password**的值替换为您的_remote_ SQL Server的值！

并将此文件添加到`.gitignore`中，不要提交到GitHub仓库。

</重要>

## 将应用程序密钥复制到Streamlit社区云

由于上面的 `secrets.toml` 文件没有提交到 GitHub，您需要将其内容单独传递给已部署的应用程序（在 Streamlit Community Cloud 上）。转到 [应用仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击 **编辑 Secrets**。将 `secrets.toml` 的内容复制到文本区域中。有关更多信息，请参阅 [Secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将pyodbc添加到您的requirements文件

要在Streamlit中与SQL Server进行本地连接，除了在SQL Server安装过程中安装的Microsoft ODBC驱动程序外，您还需要通过`pip install pyodbc`安装pyodbc。

在_Streamlit Cloud_中，我们内置了对SQL Server的支持。基于用户需求，我们直接将SQL Server工具，包括ODBC驱动程序和可执行文件 `sqlcmd` 和 `bcp`，添加到了Cloud应用的容器镜像中，因此您无需安装它们。

您只需要将 [`pyodbc`](https://github.com/mkleehammer/pyodbc) Python包添加到您的 `requirements.txt` 文件中，就可以开始使用了！🎈

```bash
# requirements.txt
pyodbc==x.x.x
```

用您想要在云上安装的pyodbc版本替换`x.x.x`☝️。

<注意>

目前，Streamlit Community Cloud不支持Azure Active Directory身份验证。当我们添加对Azure Active Directory的支持时，我们将更新本教程。

</注意>

## 编写您的Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行。确保根据您的表格名称调整`query`。

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

查看上面的 `st.cache_data`？没有它，Streamlit将在每次应用程序重新运行时运行查询（例如在小部件交互时）。使用 `st.cache_data`，它只在查询更改或10分钟后运行（这就是 `ttl` 的作用）。注意：如果您的数据库更新频率更高，您应该调整 `ttl` 或移除缓存，以便查看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们在上面创建的示例表格），您的应用程序应该是这个样子的：

![应用程序截图](/images/databases/streamlit-app.png)