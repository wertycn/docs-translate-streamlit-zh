---
slug: /knowledge-base/tutorials/databases/private-gsheet
title: Connect Streamlit to a private Google Sheet
---

# 将 Streamlit 连接到私有的 Google Sheet

## 简介

本指南将说明如何从 Streamlit Community Cloud 安全访问私有的 Google Sheet。它使用 [gsheetsdb](https://github.com/betodealmeida/gsheets-db-api) 库和 Streamlit 的 [secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management) 功能。

如果您愿意为您的Google表启用链接共享（即任何拥有链接的人都可以查看它），则指南[将Streamlit连接到公共Google表](/knowledge-base/tutorials/databases/public-gsheet)展示了一种更简单的方法。如果您的表中包含敏感信息且无法启用链接共享，请继续阅读。

## 创建一个Google表

<Note>

如果您已经有一个要使用的数据库，可以随意 [跳到下一步](#enable-the-sheets-api)。

</Note>

![Google表格截图](/images/databases/private-gsheet-1.png)

## 启用Sheets API

通过[Google Cloud Platform](https://cloud.google.com/)控制对Google Sheets的编程访问。创建一个帐户或登录并转到[**API和服务**仪表板](https://console.cloud.google.com/apis/dashboard)（如果有要求，请选择或创建一个项目）。如下所示，搜索Sheets API并启用它：

<Flex>
<Image alt="GCP截图1" src="/images/databases/private-gsheet-2.png" />
<Image alt="GCP截图2" src="/images/databases/private-gsheet-3.png" />
<Image alt="GCP截图3" src="/images/databases/private-gsheet-4.png" />
</Flex>

## 创建服务账号和密钥文件

要在Streamlit Community Cloud中使用Sheets API，您需要一个Google Cloud Platform服务帐号（一种用于编程数据访问的特殊帐号类型）。转到[**服务帐号**页面](https://console.cloud.google.com/iam-admin/serviceaccounts)并创建一个具有**Viewer**权限的帐号（这将允许帐号访问数据但不更改数据）：

<Flex>
<Image alt="GCP screenshot 5" src="/images/databases/private-gsheet-5.png" />
<Image alt="GCP截图6" src="/images/databases/private-gsheet-6.png" />
<Image alt="GCP截图7" src="/images/databases/private-gsheet-7.png" />
</Flex>

<Note>

按钮**创建服务帐号**是灰色的，表示您没有正确的权限。请向您的Google Cloud项目管理员寻求帮助。

</Note>

点击**完成**后，您应该回到服务账号概览页面。首先，记下刚刚创建的账号的电子邮件地址（对于下一步非常重要！）。然后，为新账号创建一个JSON密钥文件并下载：

<Flex>
<Image alt="GCP截图8" src="/images/databases/private-gsheet-8.png" />
<Image alt="GCP截图9" src="/images/databases/private-gsheet-9.png" />
<Image alt="GCP截图10" src="/images/databases/private-gsheet-10.png" />
</Flex>

## 将Google Sheet与服务账号共享

默认情况下，您刚刚创建的服务账号无法访问您的Google Sheet。要给它访问权限，请在Google Sheet中单击**共享**按钮，添加服务账号的电子邮件（在第2步中记录下来），并选择正确的权限（如果您只想读取数据，则**查看者**权限就足够了）：

<Flex>
<Image alt="GCP screenshot 11" src="/images/databases/private-gsheet-11.png" />
<Image alt="GCP screenshot 12" src="/images/databases/private-gsheet-12.png" />
</Flex>

## 将密钥文件添加到本地应用程序的 secrets

您的本地 Streamlit 应用程序将从位于应用程序根目录中的 `.streamlit/secrets.toml` 文件中读取密钥信息。如果该文件尚不存在，请创建该文件，并按照以下示例添加您的 Google Sheet 的 URL 和下载的密钥文件内容：

```toml
# .streamlit/secrets.toml

private_gsheets_url = "https://docs.google.com/spreadsheets/d/12345/edit?usp=sharing"

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

将此文件添加到 `.gitignore` 中，并且不要将其提交到您的 GitHub 仓库！

</重要>

## 将您的应用程序密钥复制到云端

由于上面的`secrets.toml`文件没有提交到GitHub，您需要将其内容单独传递给部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑密钥**。将`secrets.toml`的内容复制到文本区域中。有关更多信息，请参阅[Secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将 gsheetsdb 添加到您的 requirements 文件

将 [gsheetsdb](https://github.com/betodealmeida/gsheets-db-api) 包添加到您的 `requirements.txt` 文件中，最好固定其版本（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
gsheetsdb==x.x.x
```

## 编写你的 Streamlit 应用程序

复制下面的代码到你的 Streamlit 应用程序中并运行它。

```python
# streamlit_app.py

import streamlit as st
from google.oauth2 import service_account
from gsheetsdb import connect

# Create a connection object.
credentials = service_account.Credentials.from_service_account_info(
    st.secrets["gcp_service_account"],
    scopes=[
        "https://www.googleapis.com/auth/spreadsheets",
    ],
)
conn = connect(credentials=credentials)

# Perform SQL query on the Google Sheet.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def run_query(query):
    rows = conn.execute(query, headers=1)
    rows = rows.fetchall()
    return rows

sheet_url = st.secrets["private_gsheets_url"]
rows = run_query(f'SELECT * FROM "{sheet_url}"')

# Print results.
for row in rows:
    st.write(f"{row.name} has a :{row.pet}:")
```

请注意上面的`st.cache_data`吗？如果没有它，Streamlit将在每次应用重新运行时运行查询（例如在小部件交互时）。有了`st.cache_data`，它只会在查询更改或10分钟后运行（这就是`ttl`的作用）。请注意：如果您的数据库更新频率更高，您应该调整`ttl`或删除缓存，以便观看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例表格），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)