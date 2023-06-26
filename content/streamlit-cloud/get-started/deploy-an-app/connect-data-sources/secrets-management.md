---
slug: /streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management
title: Secrets management
---

# 密钥管理

## 简介

将未加密的密钥存储在git仓库中通常被认为是不良实践。如果您的应用程序需要访问敏感凭据，推荐的解决方案是将这些凭据存储在不提交到仓库的文件中，并将它们作为环境变量传递。

密钥管理允许您安全地存储密钥，并在Streamlit应用程序中将其作为环境变量访问。

## 如何使用密钥管理

### 部署应用并设置密钥

1. 访问 [http://share.streamlit.io/](http://share.streamlit.io/) 并点击 "New app" 来部署一个带有密钥的新应用。
2. 点击 "Advanced settings..."。
3. 你将看到一个弹出框，其中包含一个用于输入密钥的输入框。

   ![密钥管理](/images/streamlit-community-cloud/secrets-management.png)

4. 使用 [TOML](https://toml.io/en/latest) 格式在 "Secrets" 字段中提供您的密钥。例如：

   ```toml
   # 本节中的所有内容都将作为环境变量提供
   db_username = "Jane"
   db_password = "12345qwerty"

   # 如果您愿意，您还可以添加其他部分。
   # 如下所示的部分内容不会成为环境变量，但它们可以在Streamlit中轻松访问，
   # 正如我们在本文档后面所示。

   [my_cool_secrets]
   things_i_like = ["Streamlit", "Python"]
   ```

### 在您的应用程序中使用秘密信息

在Streamlit应用程序中，您可以通过将秘密作为环境变量或查询`st.secrets`字典来访问您的秘密。例如，如果您在上面的部分中输入了秘密，下面的代码将展示您如何在Streamlit应用程序中访问它们。

```python
import streamlit as st

# Everything is accessible via the st.secrets dict:

st.write("DB username:", st.secrets["db_username"])
st.write("DB password:", st.secrets["db_password"])
st.write("My cool secrets:", st.secrets["my_cool_secrets"]["things_i_like"])

# And the root-level secrets are also accessible as environment variables:

import os

st.write(
    "Has environment variables been set:",
    os.environ["db_username"] == st.secrets["db_username"],
)
```

<Tip>

您可以通过属性标记（例如`st.secrets.key`）访问`st.secrets`，除了键标记（例如`st.secrets["key"]`）之外，还可以像[`st.session_state`](/library/api-reference/session-state)那样。

</Tip>

您甚至可以使用TOML节来紧凑地将多个密钥作为单个属性传递。

考虑以下密钥：

```toml
[db_credentials]
username = "my_username"
password = "my_password"
```

与将每个密码作为函数的属性传递不同，您可以更紧凑地传递部分以实现相同的结果。请参考以下虚构代码，该代码使用了上面的密码:

```python
# Verbose version
my_db.connect(username=st.secrets.db_credentials.username, password=st.secrets.db_credentials.password)

# Far more compact version!
my_db.connect(**st.secrets.db_credentials)
```

### 编辑应用程序的密钥

1. 前往 [https://share.streamlit.io/](https://share.streamlit.io/)
2. 打开您的应用程序菜单，并点击 "Settings"。
   ![编辑密钥](/images/streamlit-community-cloud/edit-secrets.png)
3. 一个模态框将出现。点击 "Secrets" 部分并编辑您的密钥。
   ![编辑密钥模态框](/images/streamlit-community-cloud/edit-secrets-1.png)
4. 编辑完您的密钥后，点击"保存"。更新可能需要一段时间才能传播到您的应用程序，但当应用重新运行时，新的值将会反映出来。

### 使用密钥本地开发

在本地开发应用程序时，在应用程序存储库的根目录下创建一个名为`.streamlit`的文件夹，并在其中添加一个名为`secrets.toml`的文件，将您的密钥复制粘贴到该文件中。有关详细说明，请参阅Streamlit库的[Secrets管理](/library/advanced-features/secrets-management)文档。

<重要提示>

请确保将此文件添加到`.gitignore`中，以免提交您的密钥！

</重要提示>