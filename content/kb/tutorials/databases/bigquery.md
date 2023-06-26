---
slug: /knowledge-base/tutorials/databases/bigquery
title: Connect Streamlit to Google BigQuery
---

# 将Streamlit连接到Google BigQuery

## 简介

本指南介绍了如何从Streamlit Community Cloud安全地访问BigQuery数据库。它使用了[google-cloud-bigquery](https://googleapis.dev/python/bigquery/latest/index.html)库和Streamlit的[secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建一个BigQuery数据库

<Note>

如果您已经有一个要使用的数据库，可以随意
要[跳转到下一步](#enable-the-bigquery-api)。

</Note>

在这个示例中，我们将使用BigQuery的一个[示例数据集](https://cloud.google.com/bigquery/public-data#sample_tables)（即`shakespeare`表）。如果您想创建一个新的数据集，请按照[Google的快速入门指南](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-web-ui)进行操作。

## 启用BigQuery API

通过[Google Cloud Platform](https://cloud.google.com)可以对BigQuery进行编程访问控制。创建一个账号或登录并转到[APIs & Services仪表盘](https://console.cloud.google.com/apis/dashboard)（如果被要求选择或创建一个项目）。如下所示，搜索BigQuery API并启用它：

<Flex>
<Image alt="Bigquery截图1" src="/images/databases/big-query-1.png" />
<Image alt="Bigquery截图2" src="/images/databases/big-query-2.png" />
<Image alt="Bigquery screenshot 3" src="/images/databases/big-query-3.png" />
</Flex>

## 创建服务帐号和密钥文件

要从Streamlit Community Cloud使用BigQuery API，您需要一个Google Cloud Platform服务帐号（一种用于程序化数据访问的特殊帐户类型）。转到[服务帐号](https://console.cloud.google.com/iam-admin/serviceaccounts)页面，并创建一个具有**查看器**权限的帐户（这将允许帐户访问数据但不更改数据）：

<Flex>
<Image alt="Bigquery截图4" src="/images/databases/big-query-4.png" />
<Image alt="Bigquery截图5" src="/images/databases/big-query-5.png" />
<Image alt="Bigquery截图6" src="/images/databases/big-query-6.png" />
</Flex>

<Note>

如果按钮**创建服务帐号**为灰色，则表示您没有正确的权限。请向您的Google Cloud项目管理员寻求帮助。

</Note>

点击 **完成** 后，您应该会回到服务账户概览页面。为新账户创建一个 JSON 密钥文件并下载：

<Flex>
<Image alt="Bigquery screenshot 7" src="/images/databases/big-query-7.png" />
<Image alt="Bigquery screenshot 8" src="/images/databases/big-query-8.png" />
<Image alt="Bigquery screenshot 9" src="/images/databases/big-query-9.png" />
</Flex>

## 将密钥文件添加到本地应用程序的密钥中

您的本地Streamlit应用程序将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取密钥。如果此文件尚不存在，请创建它，并将刚刚下载的密钥文件内容添加到该文件中，如下所示：

```toml
# .streamlit/secrets.toml

[gcp_service_account]
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

<重要>

将此文件添加到`.gitignore`中，并不要将其提交到您的 GitHub 仓库！

</重要>

## 将应用程序的机密信息复制到云端

由于上面的`secrets.toml`文件没有提交到GitHub，您需要单独将其内容传递给部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，单击**编辑 Secrets**。将`secrets.toml`的内容复制到文本区域中。有关更多信息，请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![密钥管理器截图](/images/databases/edit-secrets.png)

## 将 google-cloud-bigquery 添加到您的要求文件中

将 [google-cloud-bigquery](https://googleapis.dev/python/bigquery/latest/index.html) 包添加到您的 `requirements.txt` 文件中，最好固定其版本（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
google-cloud-bigquery==x.x.x
```

## 编写您的Streamlit应用程序

将以下代码复制到您的Streamlit应用程序中并运行。如果您不使用示例表格，请确保调整查询。

```python
# streamlit_app.py

import streamlit as st
from google.oauth2 import service_account
from google.cloud import bigquery

# Create API client.
credentials = service_account.Credentials.from_service_account_info(
    st.secrets["gcp_service_account"]
)
client = bigquery.Client(credentials=credentials)

# Perform query.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def run_query(query):
    query_job = client.query(query)
    rows_raw = query_job.result()
    # Convert to list of dicts. Required for st.cache_data to hash the return value.
    rows = [dict(row) for row in rows_raw]
    return rows

rows = run_query("SELECT word FROM `bigquery-public-data.samples.shakespeare` LIMIT 10")

# Print results.
st.write("Some wise words from Shakespeare:")
for row in rows:
    st.write("✍️ " + row['word'])
```

看到上面的`st.cache_data`了吗？如果没有它，Streamlit将在每次应用程序重新运行时运行查询（例如在小部件交互时）。有了`st.cache_data`，它只会在查询更改或10分钟后运行（这就是`ttl`的作用）。请注意：如果您的数据库更新频率更高，您应该调整`ttl`或者删除缓存，以便查看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

或者，您可以使用pandas直接从BigQuery中读取数据到一个数据框中！按照上述所有步骤操作，安装[pandas-gbq](https://pandas-gbq.readthedocs.io/en/latest/index.html)库（不要忘记将其添加到`requirements.txt`中！），然后调用`pandas.read_gbq(query, credentials=credentials)`。更多信息请参考[pandas文档](https://pandas.pydata.org/docs/reference/api/pandas.read_gbq.html)。

如果一切正常（并且您使用了示例表），您的应用程序应该如下所示：

![最终应用程序截图](/images/databases/big-query-10.png)