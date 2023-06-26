---
title: 连接Streamlit到MongoDB
slug: /knowledge-base/tutorials/databases/mongodb
---

# 连接Streamlit到MongoDB

## 简介

本指南将介绍如何从Streamlit Community Cloud安全地访问一个**_远程_** MongoDB 数据库。它使用了[PyMongo](https://github.com/mongodb/mongo-python-driver)库和Streamlit的[secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建一个MongoDB数据库

<注意事项>

如果您已经有一个要使用的数据库，可以随意[跳过下一步](#add-username-and-password-to-your-local-app-secrets)。

</注意事项>

首先，按照官方教程 [安装 MongoDB](https://docs.mongodb.com/guides/server/install/)，[设置身份验证](https://docs.mongodb.com/guides/server/auth/)（记下用户名和密码！），并[连接到 MongoDB 实例](https://docs.mongodb.com/guides/server/drivers/)。一旦连接成功，打开 `mongo` shell 并输入以下两个命令来创建一个带有一些示例值的集合:

```sql
use mydb
db.mycollection.insertMany([{"name" : "Mary", "pet": "dog"}, {"name" : "John", "pet": "cat"}, {"name" : "Robert", "pet": "bird"}])
```

## 在本地应用程序中添加用户名和密码到秘密文件

您的本地Streamlit应用程序将从位于应用程序根目录下的`.streamlit/secrets.toml`文件中读取秘密信息。如果该文件尚不存在，请创建该文件并按照下面的示例添加数据库信息：

```toml
# .streamlit/secrets.toml

[mongo]
host = "localhost"
port = 27017
username = "xxx"
password = "xxx"
```

<Important>

将应用程序的密钥复制到Streamlit社区云时，请确保将**host**、**port**、**username**和**password**的值替换为您_远程_ MongoDB数据库的值！

将此文件添加到`.gitignore`中，并不要将其提交到您的GitHub仓库中！

</Important>

## 将应用程序的密钥复制到云端

由于上面的 `secrets.toml` 文件未提交到 GitHub 上，您需要将其内容分别传递给部署的应用程序（在 Streamlit Community Cloud 上）。转到 [应用仪表板](https://share.streamlit.io/)，在应用的下拉菜单中，单击 **编辑密钥**。将 `secrets.toml` 的内容复制到文本区域中。有关更多信息，请参阅 [Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将PyMongo添加到您的requirements文件中

将[PyMongo](https://github.com/mongodb/mongo-python-driver)包添加到您的`requirements.txt`文件中，最好将其版本固定（将`x.x.x`替换为您想要安装的版本）：

```bash
# requirements.txt
pymongo==x.x.x
```

## 编写您的Streamlit应用程序

将以下代码复制到您的Streamlit应用程序中并运行。确保调整您的数据库和集合的名称。

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

查看上面的`st.cache_data`？没有它，Streamlit将在每次应用程序重新运行时运行查询（例如在小部件交互时）。使用`st.cache_data`，只有在查询更改或经过10分钟后才运行（这就是`ttl`的作用）。请注意：如果您的数据库更新频率更高，您应该调整`ttl`或删除缓存，以便查看者始终看到最新数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例数据），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)
