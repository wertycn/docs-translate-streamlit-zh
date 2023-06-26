---
title: 将Streamlit连接到MySQL
slug: /knowledge-base/tutorials/databases/mysql
---

# 将Streamlit连接到MySQL

## 简介

本指南说明了如何从Streamlit Community Cloud安全访问远程MySQL数据库。它使用了[st.experimental_connection](/library/api-reference/connections/st.experimental_connection)和Streamlit的[secrets管理](/library/advanced-features/secrets-management)。下面的示例代码仅在Streamlit版本>= 1.22时才能正常工作，因为在该版本中添加了`st.experimental_connection`。

## 创建一个MySQL数据库

<Note>

如果您已经有一个要使用的数据库，可以随意跳过下一步，转到[添加用户名和密码到本地应用程序密钥](#add-username-and-password-to-your-local-app-secrets)。

首先，按照[这个教程](https://dev.mysql.com/doc/mysql-getting-started/en/)安装MySQL并启动MySQL服务器（记下用户名和密码！）。一旦您的MySQL服务器启动并运行，使用`mysql`客户端连接到它，并输入以下命令创建一个数据库和一个带有一些示例值的表：

```sql
CREATE DATABASE pets;

USE pets;

CREATE TABLE mytable (
    name            varchar(80),
    pet             varchar(80)
);

INSERT INTO mytable VALUES ('Mary', 'dog'), ('John', 'cat'), ('Robert', 'bird');
```

## 将用户名和密码添加到本地应用程序的密钥

您的本地Streamlit应用程序将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取密钥。如果该文件尚不存在，请创建它，并按照下面的示例添加您的MySQL服务器的数据库名称、用户名和密码。有关[Streamlit密钥管理](/library/advanced-features/secrets-management)的更多信息，请参阅链接。

```toml
# .streamlit/secrets.toml

[connections.mysql]
dialect = "mysql"
host = "localhost"
port = 3306
database = "xxx"
username = "xxx"
password = "xxx"
```

<重要>

当将应用程序的密钥复制到Streamlit社区云时，请确保将**host**、**port**、**database**、**username**和**password**的值替换为您的_remote_ MySQL数据库的值！

将此文件添加到`.gitignore`中，并不要将其提交到您的GitHub仓库！

</重要>

## 将应用程序的密钥复制到云端

由于上面的`secrets.toml`文件未提交到GitHub，您需要将其内容单独传递给部署的应用程序（在Streamlit社区云上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑 Secrets**。将`secrets.toml`的内容复制到文本区域中。有关更多信息，请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将依赖项添加到您的 requirements.txt 文件

在您的 `requirements.txt` 文件中添加 [mysqlclient](https://github.com/PyMySQL/mysqlclient) 和 [SQLAlchemy](https://github.com/sqlalchemy/sqlalchemy) 包，最好指定其版本（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
mysqlclient==x.x.x
SQLAlchemy==x.x.x
```

## 编写你的Streamlit应用程序

将下面的代码复制到你的Streamlit应用程序并运行。确保将`query`适应你的表的名称。

```python
# streamlit_app.py

import streamlit as st

# Initialize connection.
conn = st.experimental_connection('mysql', type='sql')

# Perform query.
df = conn.query('SELECT * from mytable;', ttl=600)

# Print results.
for row in df.itertuples():
    st.write(f"{row.name} has a :{row.pet}:")
```

查看上面的`st.experimental_connection`？它处理秘密的检索、设置、查询缓存和重试。默认情况下，`query()`的结果会被缓存而不会过期。在这种情况下，我们设置`ttl=600`以确保查询结果在不超过10分钟的时间内被缓存。您还可以设置`ttl=0`来禁用缓存。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例表格），您的应用程序应该是这样的：

![应用程序截图](/images/databases/streamlit-app.png)
