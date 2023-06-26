---
slug: /knowledge-base/deploy/authentication-without-sso
title: Authentication without SSO
---

# 无需单点登录的身份验证

## 简介

想要使用密码保护您的Streamlit应用程序，但无法实现单点登录？别担心，我们为您提供解决方案！本指南将向您展示两种简单的技术，用于向您的Streamlit应用程序添加基本身份验证功能，使用[密钥管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

<Warning>

虽然这种技术增加了一定的安全性，但它**并不**与使用SSO提供商进行适当的身份验证相媲美。

</Warning>

## 选项1：所有用户使用一个全局密码

这是最简单的选项！您的应用程序将要求使用一个在所有用户之间共享的密码。它将使用[secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)将密码存储在应用程序的秘密中。如果您想要更改此密码或撤销用户的访问权限，您需要为所有人更改密码。如果您想为每个用户设置一个密码，而不是共享一个密码，请跳转到[下面的选项2](/knowledge-base/deploy/authentication-without-sso#option-2-individual-password-for-each-user)。

### 步骤1：将密码添加到本地应用程序密钥

您的本地Streamlit应用程序将从位于应用程序根目录的文件`.streamlit/secrets.toml`中读取密钥。如果该文件尚不存在，请创建该文件，并按照下面所示添加您的密码：

```toml
# .streamlit/secrets.toml

password = "streamlit123"
```

<重要>

请确保将此文件添加到您的`.gitignore`中，以免将您的机密信息提交！

</重要>

### 第二步：将您的应用程序机密复制到云端

由于上面的 `secrets.toml` 文件并没有提交到 GitHub，您需要单独将其内容传递给部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑 Secrets**。将 `secrets.toml` 的内容复制到文本区域中。有关更多信息，请参阅[secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

### 第三步：在您的Streamlit应用程序中请求密码

将下面的代码复制到您的Streamlit应用程序中，在底部的`if`语句中插入您的正常应用程序代码，并运行它：

```python
# streamlit_app.py

import streamlit as st

def check_password():
    """Returns `True` if the user had the correct password."""

    def password_entered():
        """Checks whether a password entered by the user is correct."""
        if st.session_state["password"] == st.secrets["password"]:
            st.session_state["password_correct"] = True
            del st.session_state["password"]  # don't store password
        else:
            st.session_state["password_correct"] = False

    if "password_correct" not in st.session_state:
        # First run, show input for password.
        st.text_input(
            "Password", type="password", on_change=password_entered, key="password"
        )
        return False
    elif not st.session_state["password_correct"]:
        # Password not correct, show input + error.
        st.text_input(
            "Password", type="password", on_change=password_entered, key="password"
        )
        st.error("😕 Password incorrect")
        return False
    else:
        # Password correct.
        return True

if check_password():
    st.write("Here goes your normal Streamlit app...")
    st.button("Click me")
```

如果一切顺利，您的应用程序应该看起来像这样：

![全局密码](/images/streamlit-community-cloud/auth-without-sso-global.png)

## 选项2：每个用户的独立密码

该选项允许您为应用程序的每个用户设置用户名和密码。与[选项1](#option-1-one-global-password-for-all-users)中一样，这两个值将使用[secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)存储在应用程序的密钥管理中。

### 步骤1：将用户名和密码添加到本地应用程序密钥中

您本地的Streamlit应用程序将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取秘密信息。如果该文件不存在，请创建该文件，并按照下面所示的方式添加用户名和密码：

```toml
# .streamlit/secrets.toml

[passwords]
# Follow the rule: username = "password"
alice_foo = "streamlit123"
bob_bar = "mycrazypw"
```

<重要>

请确保将这个文件添加到你的`.gitignore`中，以免提交你的机密信息！

</重要>

或者，您可以通过电子表格或数据库设置和管理用户名和密码。要使用机密信息安全连接到Google Sheets、AWS和其他数据提供商，请阅读我们的教程，了解如何[将Streamlit连接到数据源](/knowledge-base/tutorials/databases)。

### 第二步：将你的应用程序机密信息复制到云端

由于上面的 `secrets.toml` 文件没有提交到 GitHub 中，您需要将其内容单独传递给部署的应用程序（位于 Streamlit Community Cloud 上）。前往 [应用仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击 **编辑 Secrets**。将 `secrets.toml` 的内容复制到文本区域中。有关更多信息，请参阅 [secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

### 第三步：在您的Streamlit应用程序中请求用户名和密码

将以下代码复制到您的Streamlit应用程序中，在底部的`if`语句中插入您的常规应用程序代码，并运行它：

```python
# streamlit_app.py

import streamlit as st

def check_password():
    """Returns `True` if the user had a correct password."""

    def password_entered():
        """Checks whether a password entered by the user is correct."""
        if (
            st.session_state["username"] in st.secrets["passwords"]
            and st.session_state["password"]
            == st.secrets["passwords"][st.session_state["username"]]
        ):
            st.session_state["password_correct"] = True
            del st.session_state["password"]  # don't store username + password
            del st.session_state["username"]
        else:
            st.session_state["password_correct"] = False

    if "password_correct" not in st.session_state:
        # First run, show inputs for username + password.
        st.text_input("Username", on_change=password_entered, key="username")
        st.text_input(
            "Password", type="password", on_change=password_entered, key="password"
        )
        return False
    elif not st.session_state["password_correct"]:
        # Password not correct, show input + error.
        st.text_input("Username", on_change=password_entered, key="username")
        st.text_input(
            "Password", type="password", on_change=password_entered, key="password"
        )
        st.error("😕 User not known or password incorrect")
        return False
    else:
        # Password correct.
        return True

if check_password():
    st.write("Here goes your normal Streamlit app...")
    st.button("Click me")
```

如果一切顺利，您的应用程序应该如下所示：

![各自的密码](/images/streamlit-community-cloud/auth-without-sso-individual.png)