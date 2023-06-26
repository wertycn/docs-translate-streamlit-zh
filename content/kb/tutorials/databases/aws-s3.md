---
slug: /knowledge-base/tutorials/databases/aws-s3
title: Connect Streamlit to AWS S3
---

# 将Streamlit连接到AWS S3

## 简介

本指南介绍如何从Streamlit Community Cloud安全地访问AWS S3上的文件。它使用了[Streamlit FilesConnection](https://github.com/streamlit/files-connection)、[s3fs](https://github.com/dask/s3fs)库以及可选的Streamlit的[secrets管理](/library/advanced-features/secrets-management)。

## 创建一个S3存储桶并添加文件

<注意>

如果您已经有一个想要使用的存储桶，可以随意选择
[跳转到下一步](#create-access-keys)。

</注意>

首先，[注册AWS](https://aws.amazon.com/)或登录。进入[S3控制台](https://s3.console.aws.amazon.com/s3/home)并创建一个新的存储桶：

<Flex>
<Image alt="AWS截图1" src="/images/databases/aws-1.png" />
<Image alt="AWS截图2" src="/images/databases/aws-2.png" />
</Flex>

导航到新存储桶的上传部分：

<Flex>
<Image alt="AWS截图3" src="/images/databases/aws-3.png" />
<Image alt="AWS截图4" src="/images/databases/aws-4.png" />
</Flex>

并记下“AWS区域”以备后用。在本示例中，它是`us-east-1`，但对您来说可能会有所不同。

接下来，上传包含一些示例数据的CSV文件：

<Download href="/images/databases/myfile.csv">myfile.csv</Download>

## 创建访问密钥

进入[AWS控制台](https://console.aws.amazon.com/)，按照下面的示例创建访问密钥，并复制“访问密钥ID”和“密钥访问密钥”：

<Flex>
<Image alt="AWS截图5" src="/images/databases/aws-5.png" />
<Image alt="AWS截图6" src="/images/databases/aws-6.png" />
</Flex>

<Tip>

以根用户创建的访问密钥具有广泛的权限。为了使您的AWS账户更安全，您应该考虑创建一个具有受限权限的IAM账户并使用其访问密钥。更多信息请参阅[这里](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)。

</Tip>

## 在本地设置AWS凭证

如果已经存在，Streamlit FilesConnection和s3fs将读取并使用您现有的[AWS凭证和配置](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html)，例如来自`~/.aws/credentials`文件或环境变量。

如果您还没有进行这个设置，或者计划将应用程序托管在Streamlit Community Cloud上，您应该在应用程序的根目录或您的主目录中指定来自文件`.streamlit/secrets.toml`的凭据。如果该文件还不存在，请创建它并按照以下示例添加访问密钥ID、访问密钥秘钥和您之前记录的AWS默认区域：

```toml
# .streamlit/secrets.toml
AWS_ACCESS_KEY_ID = "xxx"
AWS_SECRET_ACCESS_KEY = "xxx"
AWS_DEFAULT_REGION = "xxx"
```

<重要>

请确保将上面的`xxx`替换为您之前记录下的值，并将此文件添加到`.gitignore`中，以免将其提交到您的GitHub存储库中！

</重要>

## 将您的应用程序密钥复制到云端

要在Streamlit Community Cloud上托管您的应用程序，您需要通过secrets将您的凭据传递给已部署的应用程序。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑Secrets**。将`secrets.toml`的内容复制到文本区域中。有关更多信息，请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将 FilesConnection 和 s3fs 添加到您的 requirements 文件

将 [FilesConnection](https://github.com/streamlit/files-connection) 和 [s3fs](https://github.com/dask/s3fs) 包添加到您的 `requirements.txt` 文件中，最好针对版本进行固定（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
s3fs==x.x.x
# Direct pypi install coming soon
git+https://github.com/streamlit/files-connection
```

## 编写您的Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行。确保调整您的存储桶和文件的名称。请注意，Streamlit会自动将来自您的密钥文件的访问密钥转换为环境变量，默认情况下`s3fs`会在其中搜索它们。

```python
# streamlit_app.py

import streamlit as st
from st_files_connection import FilesConnection

# Create connection object and retrieve file contents.
# Specify input format is a csv and to cache the result for 600 seconds.
conn = st.experimental_connection('s3', type=FilesConnection)
df = conn.read("testbucket-jrieke/myfile.csv", input_format="csv", ttl=600)

# Print results.
for row in df.itertuples():
    st.write(f"{row.Owner} has a :{row.Pet}:")
```

参见上面的 `st.experimental_connection`？这处理了秘密检索、设置、结果缓存和重试。默认情况下，`read()` 的结果会被缓存而不过期。在这种情况下，我们设置 `ttl=600`，以确保文件内容的缓存时间不超过10分钟。您也可以设置 `ttl=0` 来禁用缓存。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了上面提供的示例文件），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)