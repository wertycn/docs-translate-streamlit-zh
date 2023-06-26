---
title: 连接 Streamlit 到 PostgreSQL
slug: /knowledge-base/tutorials/databases/postgresql
---

# 连接 Streamlit 到 PostgreSQL

## 简介

本指南将解释如何从 Streamlit Community Cloud 安全地访问 **_远程_** PostgreSQL 数据库。它使用 [psycopg2](https://www.psycopg.org/) 库和 Streamlit 的 [secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建 PostgreSQL 数据库

<Note>

如果您已经有一个要使用的数据库，可以随意跳过到下一步（添加用户名和密码到本地应用程序密钥）。

</注意>

首先，按照[此教程](https://www.tutorialspoint.com/postgresql/postgresql_environment.htm)安装PostgreSQL并创建一个数据库（记下数据库名称、用户名和密码！）。打开SQL Shell（`psql`）并输入以下两个命令来创建一个带有一些示例值的表：

```sql
CREATE TABLE mytable (
    name            varchar(80),
    pet             varchar(80)
);

INSERT INTO mytable VALUES ('Mary', 'dog'), ('John', 'cat'), ('Robert', 'bird');
```

## 将用户名和密码添加到本地应用程序的机密文件

您的本地Streamlit应用程序将从位于应用程序根目录中的`.streamlit/secrets.toml`文件中读取机密信息。如果该文件尚不存在，请创建该文件并按照下面的示例添加数据库的名称、用户名和密码:

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

在将应用程序密钥复制到Streamlit Community Cloud时，请确保将**主机**、**端口**、**数据库名**、**用户名**和**密码**的值替换为您的_远程_PostgreSQL数据库的值！

将此文件添加到`.gitignore`中，并不要将其提交到您的GitHub仓库中！

</重要>

## 将应用程序密钥复制到云端

由于上述的`secrets.toml`文件没有提交到GitHub中，您需要单独将其内容传递给部署的应用程序（在Streamlit社区云中）。请前往[应用仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑秘密**。将`secrets.toml`的内容复制到文本区域中。更多信息请参阅[秘密管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager截图](/images/databases/edit-secrets.png)

## 将psycopg2添加到您的requirements文件中

将[psycopg2](https://www.psycopg.org/)包添加到您的`requirements.txt`文件中，最好固定其版本（将`x.x.x`替换为您想要安装的版本）：

```bash
# requirements.txt
psycopg2-binary==x.x.x
```

## 编写您的Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行它。确保根据您的表格名称调整`query`。

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

请参考上面的`st.cache_data`。如果没有它，Streamlit将在每次应用重新运行时运行查询（例如在小部件交互时）。有了`st.cache_data`，它只在查询更改或超过10分钟后运行（这就是`ttl`的作用）。请注意：如果您的数据库更新更频繁，您应该调整`ttl`或删除缓存，以便观看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（且您使用了我们上面创建的示例表格），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)
