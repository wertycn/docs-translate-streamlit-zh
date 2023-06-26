---
slug: /knowledge-base/tutorials/databases/mongodb
title: Connect Streamlit to MongoDB
---

# 连接 Streamlit 到 MongoDB

## 简介

本指南介绍了如何从 Streamlit Community Cloud 安全地访问一个**远程** MongoDB 数据库。它使用了 [PyMongo](https://github.com/mongodb/mongo-python-driver) 库和 Streamlit 的 [secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建 MongoDB 数据库

<Note>

如果您已经有一个要使用的数据库，可以随意使用。
要[跳转到下一步](#add-username-and-password-to-your-local-app-secrets)。

</Note>

首先，请按照官方教程[安装MongoDB](https://docs.mongodb.com/guides/server/install/)，[设置身份验证](https://docs.mongodb.com/guides/server/auth/)（记下用户名和密码！），以及[连接到MongoDB实例](https://docs.mongodb.com/guides/server/drivers/)。连接成功后，打开`mongo` shell，并输入以下两个命令创建一个包含一些示例值的集合：

```sql
use mydb
db.mycollection.insertMany([{"name" : "Mary", "pet": "dog"}, {"name" : "John", "pet": "cat"}, {"name" : "Robert", "pet": "bird"}])
```

## 将用户名和密码添加到本地应用程序的秘密文件中

您的本地Streamlit应用程序将从位于应用程序根目录下的`.streamlit/secrets.toml`文件中读取秘密信息。如果该文件尚不存在，请创建它，并按照下面的示例添加数据库信息：

```toml
# .streamlit/secrets.toml

[mongo]
host = "localhost"
port = 27017
username = "xxx"
password = "xxx"
```

<重要>

在将应用程序密钥复制到Streamlit社区云时，请确保将**host**、**port**、**username**和**password**的值替换为您的_远程_ MongoDB数据库的值！

将此文件添加到`.gitignore`中，不要将其提交到您的GitHub存储库！

</重要>

## 将应用程序密钥复制到云端

由于上面的 `secrets.toml` 文件未提交到 GitHub，请单独将其内容传递给已部署的应用程序（在 Streamlit Community Cloud 上）。转到[应用仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击 **编辑 Secrets**。将 `secrets.toml` 的内容复制到文本区域中。更多信息请参阅[Secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将PyMongo添加到您的requirements文件中

将[PyMongo](https://github.com/mongodb/mongo-python-driver)包添加到您的`requirements.txt`文件中，最好指定其版本（将`x.x.x`替换为您想要安装的版本）：

```bash
# requirements.txt
pymongo==x.x.x
```

## 编写您的Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行。请确保适应您的数据库和集合的名称。

```python
# streamlit_app.py

import streamlit as st
import pymongo

# Initialize connection.
# Uses st.cache_resource to only run once.
@st.cache_resource
def init_connection():
    return pymongo.MongoClient(**st.secrets["mongo"])

client = init_connection()

# Pull data from the collection.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def get_data():
    db = client.mydb
    items = db.mycollection.find()
    items = list(items)  # make hashable for st.cache_data
    return items

items = get_data()

# Print results.
for item in items:
    st.write(f"{item['name']} has a :{item['pet']}:")
```

查看上面的`st.cache_data`？如果没有它，Streamlit将在每次应用程序重新运行时运行查询（例如在小部件交互时）。有了`st.cache_data`，它只会在查询更改或超过10分钟后运行（这就是`ttl`的作用）。注意：如果您的数据库更新频率更高，您应该调整`ttl`或删除缓存，以便观众始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例数据），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)