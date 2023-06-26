---
slug: /knowledge-base/tutorials/databases/postgresql
title: Connect Streamlit to PostgreSQL
---

# 将Streamlit连接到PostgreSQL

## 简介

本指南说明了如何从Streamlit Community Cloud安全地访问一个**_远程_**的PostgreSQL数据库。它使用了[psycopg2](https://www.psycopg.org/)库和Streamlit的[secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建一个PostgreSQL数据库

<Note>

如果您已经有一个要使用的数据库，可以随意
要[跳转到下一步](#add-username-and-password-to-your-local-app-secrets)。

</注意>

首先，按照[这个教程](https://www.tutorialspoint.com/postgresql/postgresql_environment.htm)安装PostgreSQL并创建一个数据库（记下数据库名称、用户名和密码！）。打开SQL Shell（`psql`），并输入以下两个命令来创建一个带有一些示例值的表格：

```sql
CREATE TABLE mytable (
    name            varchar(80),
    pet             varchar(80)
);

INSERT INTO mytable VALUES ('Mary', 'dog'), ('John', 'cat'), ('Robert', 'bird');
```

## 将用户名和密码添加到本地应用程序的密钥文件

您的本地Streamlit应用程序将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取密钥。如果该文件尚不存在，请创建该文件，并按照下面的示例添加数据库的名称、用户名和密码:

```toml
# .streamlit/secrets.toml

[postgres]
host = "localhost"
port = 5432
dbname = "xxx"
user = "xxx"
password = "xxx"
```

<重要>

将应用程序的密钥复制到Streamlit Community Cloud时，请确保将**host**、**port**、**dbname**、**user**和**password**的值替换为您的_remote_ PostgreSQL数据库的值！

将此文件添加到`.gitignore`中，并不要将其提交到您的GitHub仓库中！

</重要>

## 将应用程序密钥复制到云端

由于上面的 `secrets.toml` 文件没有提交到 GitHub，您需要将其内容单独传递给您部署的应用程序（在 Streamlit Community Cloud 上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**Edit Secrets**。将 `secrets.toml` 的内容复制到文本区域中。更多信息请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将 psycopg2 添加到您的 requirements 文件

将 [psycopg2](https://www.psycopg.org/) 包添加到您的 `requirements.txt` 文件中，最好固定其版本（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
psycopg2-binary==x.x.x
```

## 编写您的Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行。确保将`query`适应您的表的名称。

```python
# streamlit_app.py

import streamlit as st
import psycopg2

# Initialize connection.
# Uses st.cache_resource to only run once.
@st.cache_resource
def init_connection():
    return psycopg2.connect(**st.secrets["postgres"])

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

在上面看到了 `st.cache_data` 吗？如果没有使用它，Streamlit 将在每次应用重新运行时（例如小部件交互）都运行查询。使用 `st.cache_data`，只有在查询发生变化或者经过10分钟后（这是 `ttl` 的作用），才会运行查询。请注意：如果您的数据库更新更频繁，您应该调整 `ttl` 或者移除缓存，以便观看者始终看到最新的数据。在 [缓存](/library/advanced-features/caching) 中了解更多信息。

如果一切顺利（并且您使用了我们在上面创建的示例表），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)