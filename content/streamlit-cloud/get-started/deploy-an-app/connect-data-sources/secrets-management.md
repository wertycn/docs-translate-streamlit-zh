---
title: 密钥管理
slug: /streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management
---

# 密钥管理

## 简介

将未加密的密钥存储在git仓库中通常被认为是不良实践。如果您的应用程序需要访问敏感凭据，建议的解决方案是将这些凭据存储在不提交到仓库的文件中，并将它们作为环境变量传递。

秘密管理允许您安全地存储秘密，并在Streamlit应用程序中将其作为环境变量访问。

## 如何使用秘密管理

### 部署应用程序并设置秘密

1. 进入 [http://share.streamlit.io/](http://share.streamlit.io/)，点击“New app”来部署一个带有秘密的新应用程序。
2. 点击“Advanced settings...”。
3. 您将看到一个模态窗口，其中包含一个用于输入秘密的输入框。

   ![秘密管理](/images/streamlit-community-cloud/secrets-management.png)

4. 在 "Secrets" 字段中使用 [TOML](https://toml.io/en/latest) 格式提供您的密钥。例如：

   ```toml
   # 此部分中的所有内容都将作为环境变量可用
   db_username = "Jane"
   db_password = "12345qwerty"

   # 如果需要，您还可以添加其他部分。
   # 如下所示，部分的内容不会成为环境变量，
   # 但是它们可以在 Streamlit 内部轻松访问，如后文所示。
   [my_cool_secrets]
   things_i_like = ["Streamlit", "Python"]
   ```

### 在应用程序中使用密钥

可以将密钥作为环境变量或通过查询`st.secrets`字典来访问。例如，如果您在上面的部分输入了密钥，下面的代码将展示您如何在Streamlit应用程序中访问它们。

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

您可以通过属性表示法（例如`st.secrets.key`）访问`st.secrets`，除了键表示法（例如`st.secrets["key"]`）之外，还可以像[`st.session_state`](/library/api-reference/session-state)一样。

</Tip>

您甚至可以使用TOML节来将多个密钥作为单个属性传递。

考虑以下密钥：

```toml
[db_credentials]
username = "my_username"
password = "my_password"
```

与将每个密钥作为函数中的属性传递不同，您可以更紧凑地传递部分以实现相同的结果。请参阅以下示例代码，其中使用了上面的密钥:

```python
# Verbose version
my_db.connect(username=st.secrets.db_credentials.username, password=st.secrets.db_credentials.password)

# Far more compact version!
my_db.connect(**st.secrets.db_credentials)
```

### 编辑应用程序的密钥

1. 前往 [https://share.streamlit.io/](https://share.streamlit.io/)
2. 打开您的应用程序菜单，点击 "设置"。
   ![编辑密钥](/images/streamlit-community-cloud/edit-secrets.png)
3. 会出现一个模态窗口。点击 "密钥" 部分并编辑您的密钥。
   ![编辑密钥模态窗口](/images/streamlit-community-cloud/edit-secrets-1.png)
4. 在编辑完您的密钥后，点击“保存”。更新可能需要一些时间才能传播到您的应用程序，但在应用程序重新运行时，新的值将会被反映出来。

### 使用密钥在本地开发

在本地开发应用程序时，在应用程序仓库的根目录下的一个名为`.streamlit`的文件夹中添加一个名为`secrets.toml`的文件，并将您的秘密复制粘贴到该文件中。有关详细说明，请参阅Streamlit库的[Secrets管理](/library/advanced-features/secrets-management)文档。

<重要提示>

请确保将此文件添加到您的`.gitignore`中，以免将您的秘密提交到版本控制系统中！

</重要提示>
