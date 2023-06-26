---
slug: /library/advanced-features/secrets-management
title: Secrets management
---

# 密钥管理

在git仓库中存储未加密的密钥是一种不好的做法。对于需要访问敏感凭据的应用程序，推荐的解决方案是将这些凭据存储在仓库之外 - 例如使用不提交到仓库的凭据文件或将它们作为环境变量传递。

Streamlit提供本地基于文件的密钥管理功能，可以轻松存储和安全访问您在Streamlit应用程序中使用的密钥。

<注意>

现有的密钥管理工具，如[dotenv文件](https://pypi.org/project/python-dotenv/)、[AWS凭证文件](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html#configuring-credentials)、[Google Cloud Secret Manager](https://pypi.org/project/google-cloud-secret-manager/)或[Hashicorp Vault](https://www.vaultproject.io/use-cases/secrets-management)，在Streamlit中都可以正常工作。我们只是在需要时添加本地的密钥管理功能。

</Note>

## 如何使用密钥管理

### 本地开发和设置密钥

Streamlit提供了两种使用[TOML](https://toml.io/en/latest)格式在本地设置密钥的方法：

1. 在**全局密钥文件**中位于`~/.streamlit/secrets.toml`（macOS/Linux）或`%userprofile%/.streamlit/secrets.toml`（Windows）：

   ```toml
   # 此部分中的所有内容将作为环境变量可用
   db_username = "Jane"
   db_password = "mypassword"

   # 如果您喜欢，还可以添加其他部分。
   # 如下所示，部分的内容不会成为环境变量，
   # 但是它们在Streamlit内部仍然可以轻松访问，我们在本文档中会展示。
   [my_other_secrets]
   things_i_like = ["Streamlit", "Python"]
   ```

   如果您使用全局的secrets文件，那么如果多个Streamlit应用程序共享相同的secrets，就不需要在多个项目级别文件中复制secrets。

2. 在项目的**私密文件**中，路径为 `$CWD/.streamlit/secrets.toml`，其中 `$CWD` 是您运行 Streamlit 的文件夹。如果同时存在全局的私密文件和项目私密文件，则项目私密文件中的私密信息会覆盖全局文件中的私密信息。

<重要提示>

请将此文件添加到您的 `.gitignore` 中，以免将私密信息提交到版本控制！

</重要提示>

### 在您的应用程序中使用私密信息

通过查询`st.secrets`字典或作为环境变量来访问您的秘密。例如，如果您在上面的部分中输入了秘密，下面的代码将向您展示如何在Streamlit应用程序中访问它们。

```python
import streamlit as st

# Everything is accessible via the st.secrets dict:

st.write("DB username:", st.secrets["db_username"])
st.write("DB password:", st.secrets["db_password"])

# And the root-level secrets are also accessible as environment variables:

import os

st.write(
    "Has environment variables been set:",
    os.environ["db_username"] == st.secrets["db_username"],
)
```

<Tip>

您可以通过属性表示法（例如`st.secrets.key`）访问`st.secrets`，除了键表示法（例如`st.secrets["key"]`）之外，还可以使用[st.session_state](/library/api-reference/session-state)的方式。

</Tip>

您甚至可以使用TOML部分来以单个属性传递多个密钥。考虑以下密钥：

```toml
[db_credentials]
username = "my_username"
password = "my_password"
```

与将每个密钥作为函数属性传递不同，您可以通过传递部分来更紧凑地实现相同的结果。请参见下面的示例代码，其中使用了上面的密钥：

```python
# Verbose version
my_db.connect(username=st.secrets.db_credentials.username, password=st.secrets.db_credentials.password)

# Far more compact version!
my_db.connect(**st.secrets.db_credentials)
```

### 错误处理

以下是在使用密钥管理时可能遇到的一些常见错误。

- 如果在应用程序运行时创建了 `.streamlit/secrets.toml` 文件，则需要重新启动服务器才能在应用程序中反映出更改。
- 如果尝试访问一个密钥，但没有 `secrets.toml` 文件存在，Streamlit 将引发 `FileNotFoundError` 异常:
  <Image alt="Secrets management FileNotFoundError" src="/images/secrets-filenotfounderror.png" clean />
- 如果尝试访问不存在的密码，Streamlit将引发`KeyError`异常：

  ```python
  import streamlit as st

  st.write(st.secrets["nonexistent_key"])
  ```

  <Image alt="Secrets management KeyError" src="/images/secrets-keyerror.png" clean />

### 在Streamlit Community Cloud上使用密码

在将您的应用部署到[Streamlit社区云](https://streamlit.io/cloud)时，您可以使用与本地相同的密钥管理工作流程。但是，您还需要在社区云密钥管理控制台中设置您的密钥。通过云特定的[密钥管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)文档来了解如何设置。