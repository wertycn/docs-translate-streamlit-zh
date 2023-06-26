---
title: 密钥管理
slug: /library/advanced-features/secrets-management
---

# 密钥管理

将未加密的密钥存储在Git存储库中是一种不好的做法。对于需要访问敏感凭据的应用程序，推荐的解决方案是将这些凭据存储在存储库之外，例如使用不提交到存储库的凭据文件或将其作为环境变量传递。

Streamlit 提供本地文件的密钥管理功能，可以方便地存储和安全地访问您的 Streamlit 应用程序中的密钥。

<注>

现有的秘密管理工具，例如[dotenv文件](https://pypi.org/project/python-dotenv/)、[AWS凭据文件](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html#configuring-credentials)、[Google Cloud Secret Manager](https://pypi.org/project/google-cloud-secret-manager/)或[Hashicorp Vault](https://www.vaultproject.io/use-cases/secrets-management)，在Streamlit中可以很好地工作。我们只是在需要时添加本地的秘密管理功能。

</Note>

## 如何使用密钥管理

### 本地开发和设置密钥

Streamlit 提供了两种使用 [TOML](https://toml.io/en/latest) 格式在本地设置密钥的方法:

1. 在位于 `~/.streamlit/secrets.toml`（macOS/Linux）或 `%userprofile%/.streamlit/secrets.toml`（Windows） 的 **全局密钥文件** 中:

   ```toml
   # 此部分的所有内容将作为环境变量提供
   db_username = "Jane"
   db_password = "mypassword"

   # 如果您喜欢，您还可以添加其他部分。
 # 如下所示的各节的内容不会成为环境变量，但它们仍然可以在Streamlit中轻松访问，正如我们在本文档中稍后所示。

 [my_other_secrets]
 things_i_like = ["Streamlit", "Python"]
 ```

 如果您使用全局机密文件，那么如果多个Streamlit应用程序共享相同的机密信息，您就不需要在多个项目级文件中重复添加机密信息。

2. 在**每个项目的密钥文件**中，位于`$CWD/.streamlit/secrets.toml`，其中`$CWD`是您运行Streamlit的文件夹。如果全局密钥文件和每个项目的密钥文件都存在，那么在每个项目的文件中定义的密钥将覆盖全局文件中的密钥。

<重要提示>

请将此文件添加到`.gitignore`中，以免将您的密钥提交到代码库！

</重要提示>

### 在您的应用程序中使用密钥

通过查询 `st.secrets` 字典或作为环境变量来访问您的密钥。例如，如果您输入了上面部分的密钥，下面的代码将展示如何在您的Streamlit应用程序中访问它们。

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

您可以通过属性访问符号（例如 `st.secrets.key`）来访问 `st.secrets`，除了键符号访问（例如 `st.secrets["key"]`），就像 [st.session_state](/library/api-reference/session-state)一样。

</Tip>

您甚至可以使用TOML节来将多个密钥作为单个属性传递。考虑以下密钥：

```toml
[db_credentials]
username = "my_username"
password = "my_password"
```

与将每个密钥作为函数属性传递不同，您可以更紧凑地传递部分密钥以达到相同的结果。请参见下面的示例代码，该代码使用了上面的密钥:

```python
# Verbose version
my_db.connect(username=st.secrets.db_credentials.username, password=st.secrets.db_credentials.password)

# Far more compact version!
my_db.connect(**st.secrets.db_credentials)
```

### 错误处理

以下是在使用秘密管理时可能遇到的一些常见错误。

- 如果在应用程序运行时创建了`.streamlit/secrets.toml`文件，则需要重新启动服务器以使更改在应用程序中生效。
- 如果您尝试访问一个秘密，但没有`secrets.toml`文件存在，Streamlit将引发`FileNotFoundError`异常：
  <Image alt="Secrets management FileNotFoundError" src="/images/secrets-filenotfounderror.png" clean />
- 如果尝试访问不存在的密钥，Streamlit将引发`KeyError`异常：

  ```python
  import streamlit as st

  st.write(st.secrets["nonexistent_key"])
  ```

    <Image alt="Secrets management KeyError" src="/images/secrets-keyerror.png" clean />

### 在Streamlit社区云上使用密钥

当您将应用程序部署到[Streamlit Community Cloud](https://streamlit.io/cloud)时，您可以使用与本地相同的密钥管理工作流程。但是，您还需要在Community Cloud Secrets Management控制台中设置您的密钥。请参阅云特定的[Secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)文档，了解如何进行设置。
