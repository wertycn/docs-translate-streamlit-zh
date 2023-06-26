---
标题: 将 Streamlit 连接到 AWS S3
别名: /knowledge-base/tutorials/databases/aws-s3
---

# 将 Streamlit 连接到 AWS S3

## 简介

本指南介绍了如何从 Streamlit Community Cloud 安全地访问 AWS S3 上的文件。它使用了 [Streamlit FilesConnection](https://github.com/streamlit/files-connection)、[s3fs](https://github.com/dask/s3fs) 库和可选的 Streamlit 的 [secrets 管理](/library/advanced-features/secrets-management) 功能。

## 创建一个 S3 存储桶并添加一个文件

<注意>

如果您已经有一个要使用的存储桶，可以随意跳过下一步"创建访问密钥"。

</注意>

首先，[注册AWS](https://aws.amazon.com/)或登录。进入[S3控制台](https://s3.console.aws.amazon.com/s3/home)并创建一个新的存储桶：

<Flex>
<Image alt="AWS截图1" src="/images/databases/aws-1.png" />
<Image alt="AWS截图2" src="/images/databases/aws-2.png" />
</Flex>

转到新存储桶的上传部分：

<Flex>
<Image alt="AWS截图3" src="/images/databases/aws-3.png" />
<Image alt="AWS截图4" src="/images/databases/aws-4.png" />
</Flex>

并记下稍后使用的 "AWS区域"。例如，在此示例中，它是 `us-east-1`，但对您而言可能会有所不同。

接下来，上传包含一些示例数据的CSV文件：

<Download href="/images/databases/myfile.csv">myfile.csv</Download>

## 创建访问密钥

转到[AWS控制台](https://console.aws.amazon.com/)，按照下面的步骤创建访问密钥，并复制“访问密钥ID”和“秘密访问密钥”：

<Flex>
<Image alt="AWS截图5" src="/images/databases/aws-5.png" />
<Image alt="AWS截图6" src="/images/databases/aws-6.png" />
</Flex>

<Tip>

作为根用户创建的访问密钥具有广泛的权限。为了保护您的AWS账户的安全性，建议创建一个IAM用户并为其分配适当的权限。
为了更安全，您应该考虑创建一个具有受限权限的IAM账户，并使用其访问密钥。更多信息请参考[这里](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)。

</Tip>

## 在本地设置您的AWS凭证

Streamlit的FilesConnection和s3fs将读取并使用您现有的[AWS凭证和配置](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html)，如果可用的话，例如来自`~/.aws/credentials`文件或环境变量。

如果您还没有进行此设置，或者计划将应用程序托管在Streamlit社区云上，您应该在应用程序的根目录或您的主目录中指定来自文件`.streamlit/secrets.toml`的凭据。如果该文件尚不存在，请创建该文件并将访问密钥ID、访问密钥秘钥以及您之前记录的AWS默认区域添加到其中，如下所示：

```toml
# .streamlit/secrets.toml
AWS_ACCESS_KEY_ID = "xxx"
AWS_SECRET_ACCESS_KEY = "xxx"
AWS_DEFAULT_REGION = "xxx"
```

<重要>

请确保将上面的 `xxx` 替换为您之前记录下来的值，并将此文件添加到 `.gitignore` 中，以免将其提交到您的 GitHub 仓库中！

</重要>

## 将您的应用程序密钥复制到云端

要在Streamlit Community Cloud上托管您的应用程序，您需要通过secrets将您的凭据传递给已部署的应用程序。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，单击**编辑Secrets**。将`secrets.toml`的内容复制到文本区域中。更多信息请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 添加 FilesConnection 和 s3fs 到您的 requirements 文件

将 [FilesConnection](https://github.com/streamlit/files-connection) 和 [s3fs](https://github.com/dask/s3fs) 包添加到您的 `requirements.txt` 文件中，最好固定版本（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
s3fs==x.x.x
# Direct pypi install coming soon
git+https://github.com/streamlit/files-connection
```

## 编写你的Streamlit应用程序

将下面的代码复制到你的Streamlit应用程序中并运行。请确保调整您的存储桶和文件的名称。请注意，Streamlit会自动将您的密钥文件中的访问密钥转换为环境变量，默认情况下`s3fs`会在这些环境变量中搜索它们。

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

请参考上面的 `st.experimental_connection`。它处理秘密信息的检索、设置、结果缓存和重试。默认情况下，`read()` 的结果会被缓存而不过期。在这个例子中，我们设置 `ttl=600` 来确保文件内容最多被缓存10分钟。您还可以设置 `ttl=0` 来禁用缓存。在[Caching](/library/advanced-features/caching)中了解更多。

如果一切顺利（并且您使用了上面给出的示例文件），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)
