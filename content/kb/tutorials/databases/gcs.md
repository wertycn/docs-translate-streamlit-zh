---
slug: /knowledge-base/tutorials/databases/gcs
title: Connect Streamlit to Google Cloud Storage
---

# 将 Streamlit 连接到 Google Cloud Storage

## 简介

本指南介绍如何从 Streamlit Community Cloud 安全访问 Google Cloud Storage 上的文件。它使用了 [Streamlit FilesConnection](https://github.com/streamlit/files-connection)、[gcsfs](https://github.com/fsspec/gcsfs) 库和 Streamlit 的 [secrets management](/library/advanced-features/secrets-management) 功能。

## 创建 Google Cloud Storage 存储桶并添加文件

<Note>

如果您已经有一个要使用的存储桶，请随意跳到下一步[启用Google Cloud Storage API](#enable-the-google-cloud-storage-api)。

</Note>

首先，[注册Google Cloud Platform](https://console.cloud.google.com/)或登录。进入[Google Cloud Storage控制台](https://console.cloud.google.com/storage/)并创建一个新的存储桶。

<Flex>
<Image alt="GCS截图1" src="/images/databases/gcs-1.png" />
<Image alt="GCS截图2" src="/images/databases/gcs-2.png" />
</Flex>

转到新桶的上传部分：

<Flex>
<Image alt="GCS截图3" src="/images/databases/gcs-3.png" />
<Image alt="GCS截图4" src="/images/databases/gcs-4.png" />
</Flex>

然后上传包含一些示例数据的以下CSV文件：

<Download href="/images/databases/myfile.csv">myfile.csv</Download>

## 启用Google Cloud Storage API

当您通过Google Cloud Console或CLI创建项目时，默认情况下会启用Google Cloud Storage API。请随意跳转至下一步创建服务帐号和密钥文件。

如果您确实需要在项目中启用API以便进行编程访问，请前往[API和服务仪表板](https://console.cloud.google.com/apis/dashboard)（如果需要，请选择或创建一个项目）。搜索Cloud Storage API并启用它。下图显示了一个蓝色的“管理”按钮，并指示“API已启用”，这意味着无需采取进一步的操作。这很可能是您的情况，因为API默认已启用。但是，如果您看到的不是这样的界面，而是一个“启用”按钮，您将需要启用该API：

<Flex>
<Image alt="GCS截图5" src="/images/databases/gcs-5.png" />
<Image alt="GCS截图6" src="/images/databases/gcs-6.png" />
<Image alt="GCS截图7" src="/images/databases/gcs-7.png" />
</Flex>

## 创建服务账号和密钥文件

要在Streamlit中使用Google Cloud Storage API，您需要一个Google Cloud Platform服务账号（用于程序化数据访问的特殊类型账号）。请前往服务账号页面创建一个具有<b>查看者</b>权限的账号。

<Flex>
<Image alt="GCS截图8" src="/images/databases/gcs-8.png" />
<Image alt="GCS截图9" src="/images/databases/gcs-9.png" />
<Image alt="GCS截图10" src="/images/databases/gcs-10.png" />
</Flex>

<Note>

如果按钮 **创建服务帐号** 是灰色的，说明您没有正确的权限。请向您的Google Cloud项目管理员寻求帮助。

</Note>

点击 **完成** 后，您应该回到服务账号概览页面。为新账号创建一个 JSON 密钥文件并下载：

<Flex>
<Image alt="GCS 截图 11" src="/images/databases/gcs-11.png" />
<Image alt="GCS 截图 12" src="/images/databases/gcs-12.png" />
<Image alt="GCS 截图 13" src="/images/databases/gcs-13.png" />
</Flex>

## 将密钥添加到本地应用程序的密钥库中

您的本地Streamlit应用程序将从位于应用程序根目录的`.streamlit/secrets.toml`文件中读取密钥。如果该文件尚不存在，请创建该文件，并按照下面的示例将访问密钥添加到其中：

```toml
# .streamlit/secrets.toml

[connections.gcs]
type = "service_account"
project_id = "xxx"
private_key_id = "xxx"
private_key = "xxx"
client_email = "xxx"
client_id = "xxx"
auth_uri = "https://accounts.google.com/o/oauth2/auth"
token_uri = "https://oauth2.googleapis.com/token"
auth_provider_x509_cert_url = "https://www.googleapis.com/oauth2/v1/certs"
client_x509_cert_url = "xxx"
```

<Important>

将此文件添加到`.gitignore`中，并且不要将其提交到您的GitHub仓库！

</Important>

## 将应用程序的秘密复制到云端

由于上面的 `secrets.toml` 文件没有提交到 GitHub，您需要单独将其内容传递给部署的应用程序（在 Streamlit Community Cloud 上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击 **编辑 Secrets**。将 `secrets.toml` 的内容复制到文本区域中。更多信息请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 向您的requirements文件中添加FilesConnection和gcsfs

将[FilesConnection](https://github.com/streamlit/files-connection)和[gcsfs](https://github.com/fsspec/gcsfs)包添加到您的`requirements.txt`文件中，最好使用版本锁定（将`x.x.x`替换为您想要安装的版本）：

```bash
# requirements.txt
gcsfs==x.x.x
# Direct pypi install coming soon
git+https://github.com/streamlit/files-connection
```

## 编写你的Streamlit应用程序

将以下代码复制到你的Streamlit应用程序并运行。确保调整你的存储桶和文件的名称。请注意，Streamlit会自动将你的凭证文件中的访问密钥转换为环境变量。

```python
# streamlit_app.py

import streamlit as st
from st_files_connection import FilesConnection

# Create connection object and retrieve file contents.
# Specify input format is a csv and to cache the result for 600 seconds.
conn = st.experimental_connection('gcs', type=FilesConnection)
df = conn.read("streamlit-bucket/myfile.csv", input_format="csv", ttl=600)

# Print results.
for row in df.itertuples():
    st.write(f"{row.Owner} has a :{row.Pet}:")
```

查看上面的 `st.experimental_connection` 吗？它处理了密钥的获取、设置、结果缓存和重试。默认情况下，`read()` 的结果会被缓存且不会过期。在这种情况下，我们设置 `ttl=600` 来确保文件内容在不超过10分钟后被缓存。您也可以设置 `ttl=0` 来禁用缓存。在 [缓存](/library/advanced-features/caching) 中了解更多信息。

如果一切都顺利（并且您使用了上面给出的示例文件），您的应用程序应该看起来像这样：

![应用程序截图](/images/databases/streamlit-app.png)