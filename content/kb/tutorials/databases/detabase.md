---
title: 将Streamlit连接到Deta数据库
slug: /knowledge-base/tutorials/databases/deta-base
---

# 将Streamlit连接到Deta数据库

## 简介

本指南介绍了如何从Streamlit Community Cloud安全访问和写入[Deta Base](https://deta.space/docs/en/reference/base/about)数据库。Deta Base是一个完全托管且快速的NoSQL数据库，注重终端用户的简易性。数据存储在您自己的[Deta Space](https://deta.space/developers)的"个人云"中。本指南使用[Deta Python SDK](https://github.com/deta/deta-python)进行Deta Base和Streamlit的[Secrets Management](https://www.notion.so/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建账户并登录到Deta Space

首先，您需要[创建一个Deta Space账户](https://deta.space/signup?dev_mode=true)以使用Deta Base。在注册时，请确保启用了"开发者模式"选项。一旦您拥有了一个账户，请登录到Deta Space。登录后，点击打开Collections应用程序。

[Deta Collections](https://deta.space/manual/features/collections#what-are-collections)是Space上预安装的应用程序，用于存储不同类型的数据，可以与其他应用程序或服务进行连接。
<Image alt="Deta Space Canvas" src="/images/databases/deta-1.png" caption="Deta Space Canvas" />

现在点击**开始**按钮，然后在为您的Collection命名后点击**创建Collection**按钮。
在此之后，点击页面右上角的**Collection Settings**选项，将显示用于创建**Data Key**的模态框。点击**Create New Data Key**按钮，然后给你的密钥命名，并点击**Generate**按钮。通过点击复制按钮将密钥复制到剪贴板中。

[数据密钥](https://deta.space/docs/en/basics/extending_apps#data-keys) 允许您在集合中读取和操作数据。
<Image alt="生成数据密钥" src="/images/databases/deta-3.png" caption="生成数据密钥" />

请确保安全地存储您的**数据密钥**。它只会显示一次，您将需要它来连接到您的Deta Base。

## 将数据密钥添加到本地应用程序的密钥中

您的本地Streamlit应用程序将从您应用程序的根目录下的`.streamlit/secrets.toml`文件中读取密钥。如果该文件尚不存在，请创建该文件，并按照以下示例添加您的Deta数据库的**数据密钥**（来自上一步骤）:

```toml
# .streamlit/secrets.toml
data_key = "xxx"
```

请使用上面☝️的`xxx`替换为您在之前步骤中获得的**数据密钥**。

<重要>

将此文件添加到`.gitignore`中，不要将其提交到您的GitHub仓库！

</重要>

## 将您的应用程序密钥复制到云端

由于上面的`secrets.toml`文件没有提交到GitHub，您需要将其内容单独传递给部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑 Secrets**。将`secrets.toml`的内容复制到文本区域中。更多信息请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将deta添加到您的requirements文件中

将[Deta](https://github.com/deta/deta-python) Python SDK for Deta Base添加到您的`requirements.txt`文件中，最好固定其版本（将`x.x.x`替换为您想要安装的版本）：

```bash
# requirements.txt
deta==x.x.x
```

## 编写Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序并运行。下面的示例应用程序将从Streamlit表单将数据写入Deta Base数据库`example-db`。

```python
import streamlit as st
from deta import Deta

# Data to be written to Deta Base
with st.form("form"):
    name = st.text_input("Your name")
    age = st.number_input("Your age")
    submitted = st.form_submit_button("Store in database")


# Connect to Deta Base with your Data Key
deta = Deta(st.secrets["data_key"])

# Create a new database "example-db"
# If you need a new database, just use another name.
db = deta.Base("example-db")

# If the user clicked the submit button,
# write the data from the form to the database.
# You can store any data you want here. Just modify that dictionary below (the entries between the {}).
if submitted:
    db.put({"name": name, "age": age})

"---"
"Here's everything stored in the database:"
# This reads all items from the database and displays them to your app.
# db_content is a list of dictionaries. You can do everything you want with it.
db_content = db.fetch().items
st.write(db_content)
```

如果一切顺利（并且您使用了我们上面创建的示例），您的应用程序应该如下所示：

![应用程序完成的GIF](/images/databases/deta_app.gif)
