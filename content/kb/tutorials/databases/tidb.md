---
slug: /knowledge-base/tutorials/databases/tidb
title: Connect Streamlit to TiDB
---

# 将Streamlit连接到TiDB

## 简介

本指南介绍如何从Streamlit Community Cloud安全地访问远程的TiDB数据库。它使用了[st.experimental_connection](/library/api-reference/connections/st.experimental_connection)和Streamlit的[secrets管理](/library/advanced-features/secrets-management)。下面的示例代码仅在Streamlit版本大于等于1.22时可用，因为在这个版本中添加了`st.experimental_connection`。

[TiDB](https://www.pingcap.com/tidb/) 是一个开源的、兼容MySQL的数据库，支持混合事务和分析处理（HTAP）工作负载。[TiDB Cloud](https://www.pingcap.com/tidb-cloud/) 是一个完全托管的云数据库服务，为开发人员简化了TiDB数据库的部署和管理。

## 登录 TiDB Cloud 并创建一个集群

首先，前往[TiDB Cloud](https://tidbcloud.com/free-trial)注册一个免费帐户，您可以使用Google、GitHub、Microsoft或电子邮件进行注册：

![注册TiDB Cloud](/images/databases/tidb-1.png)

成功登录后，您已经拥有一个TiDB集群：

![集群列表](/images/databases/tidb-2.png)

如果需要，您可以创建更多的集群。点击集群名称进入集群概览页面：

![集群概览](/images/databases/tidb-3.png)

然后单击**连接**以轻松获取访问集群的连接参数。在弹出窗口中，单击**创建密码**来设置密码。

![获取连接参数](/images/databases/tidb-4.png)

<重要>

请确保记下密码。在此步骤之后，密码将无法在TiDB Cloud上获取。

</重要>

## 创建一个TiDB数据库

<注意>

如果您已经有一个要使用的数据库，可以随意使用它，否则请按照以下步骤创建一个新的TiDB数据库。

</注意>
要[跳转到下一步](#add-username-and-password-to-your-local-app-secrets)。

</Note>

一旦您的TiDB集群启动并运行，使用`mysql`客户端（或在控制台上的**Chat2Query**选项卡中）连接到它，并输入以下命令创建一个带有一些示例值的数据库和表：

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

您的本地Streamlit应用程序将从应用程序根目录中的文件`.streamlit/secrets.toml`中读取密钥。如果该文件尚不存在，请创建它，并按照下面的示例添加您的TiDB集群的主机、用户名和密码。有关[Streamlit密钥管理](/library/advanced-features/secrets-management)的更多信息，请参阅文档。

```toml
# .streamlit/secrets.toml

[connections.tidb]
dialect = "mysql"
host = "<TiDB_cluster_host>"
port = 4000
database = "pets"
username = "<TiDB_cluster_user>"
password = "<TiDB_cluster_password>"
```

<重要>

在将应用程序密钥复制到Streamlit Community Cloud时，请确保将**host**、**username**和**password**的值替换为您的 _远程_ TiDB 集群的值！

将此文件添加到`.gitignore`中，并且不要将其提交到您的 GitHub 存储库中！

</重要>

## 将应用程序密钥复制到云端

由于上面的`secrets.toml`文件没有提交到GitHub，您需要单独将其内容传递给已部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑 Secrets**。将`secrets.toml`的内容复制到文本区域中。更多信息请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将依赖项添加到您的`requirements.txt`文件中

将[mysqlclient](https://github.com/PyMySQL/mysqlclient)和[SQLAlchemy](https://github.com/sqlalchemy/sqlalchemy)包添加到您的`requirements.txt`文件中，最好指定其版本（将`x.x.x`替换为您想要安装的版本）：

```bash
# requirements.txt
mysqlclient==x.x.x
SQLAlchemy==x.x.x
```

## 编写您的Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行。确保根据您的表格名称来调整`query`的使用。

```python
# streamlit_app.py

import streamlit as st

# Initialize connection.
conn = st.experimental_connection('tidb', type='sql')

# Perform query.
df = conn.query('SELECT * from mytable;', ttl=600)

# Print results.
for row in df.itertuples():
    st.write(f"{row.name} has a :{row.pet}:")
```

请参阅上面的 `st.experimental_connection`。它处理密钥的获取、设置、查询缓存和重试。默认情况下，`query()` 的结果将被缓存且不会过期。在这种情况下，我们将 `ttl=600` 设置为确保查询结果被缓存的时间不超过10分钟。您也可以将 `ttl=0` 设置为禁用缓存。了解更多信息，请查看[Caching](/library/advanced-features/caching)。

如果一切顺利（并且您使用了我们上面创建的示例表），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)

## 使用PyMySQL连接

除了[mysqlclient](https://github.com/PyMySQL/mysqlclient)之外，[PyMySQL](https://github.com/PyMySQL/PyMySQL)是另一个流行的MySQL Python客户端。要使用PyMySQL，首先需要调整您的要求文件：

```bash
# requirements.txt
PyMySQL==x.x.x
SQLAlchemy==x.x.x
```

然后调整您的密钥文件:

```toml
# .streamlit/secrets.toml

[connections.tidb]
dialect = "mysql"
driver = "pymysql"
host = "<TiDB_cluster_host>"
port = 4000
database = "pets"
username = "<TiDB_cluster_user>"
password = "<TiDB_cluster_password>"
create_engine_kwargs = { connect_args = { ssl = { ca = "<path_to_CA_store>" }}}
```