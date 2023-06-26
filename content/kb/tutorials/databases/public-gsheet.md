---
title: 将 Streamlit 连接到公共 Google Sheet
slug: /knowledge-base/tutorials/databases/public-gsheet
---

# 将 Streamlit 连接到公共 Google Sheet

## 简介

本指南介绍了如何从 Streamlit Community Cloud 安全访问公共 Google Sheet。它使用了 [pandas](https://pandas.pydata.org/) 库和 Streamlit 的 [secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management) 功能。

该方法要求您启用Google Sheet的链接共享。虽然共享链接不会出现在您的代码中（实际上起到了一种密码的作用！），但拥有该链接的人可以获取表格中的所有数据。如果您不希望如此，请按照（更复杂的）指南[将Streamlit连接到私有Google Sheet](private-gsheet)。

## 创建一个Google Sheet并启用链接共享

<Note>

如果您已经有一个要访问的表格，可以随意[跳到下一个步骤]
[添加 Sheets URL 到本地应用程序的秘钥文件](#add-the-sheets-url-to-your-local-app-secrets)。

</注意>

<灵活布局>
<图像 alt="截图 1" src="/images/databases/public-gsheet-1.png" />
<图像 alt="截图 2" src="/images/databases/public-gsheet-2.png" />
</灵活布局>

## 将 Sheets URL 添加到本地应用程序的秘钥文件中

您本地的 Streamlit 应用程序将从位于应用程序根目录下的文件 `.streamlit/secrets.toml` 中读取秘钥。如果该文件尚不存在，请创建此文件，并将您的 Google Sheet 的共享链接添加到其中，如下所示：

```toml
# .streamlit/secrets.toml

public_gsheets_url = "https://docs.google.com/spreadsheets/d/xxxxxxx/edit#gid=0"
```

<重要>

将此文件添加到`.gitignore`中，并且不要将其提交到您的GitHub仓库中！

</重要>

## 将应用程序的密钥复制到云端

由于上面的`secrets.toml`文件未提交到GitHub，您需要将其内容单独传递给部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑 Secrets**。将`secrets.toml`的内容复制到文本区域中。更多信息请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 编写您的 Streamlit 应用程序

将以下代码复制到您的 Streamlit 应用程序中并运行它。

```python
# streamlit_app.py

import pandas as pd
import streamlit as st

# Read in data from the Google Sheet.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def load_data(sheets_url):
    csv_url = sheets_url.replace("/edit#gid=", "/export?format=csv&gid=")
    return pd.read_csv(csv_url)

df = load_data(st.secrets["public_gsheets_url"])

# Print results.
for row in df.itertuples():
    st.write(f"{row.name} has a :{row.pet}:")
```

请查看上面的 `st.cache_data` ？如果没有它，Streamlit 将在每次应用程序重新运行时运行查询（例如在小部件交互时）。使用 `st.cache_data`，它只在查询更改或经过10分钟后运行（这就是 `ttl` 的作用）。请注意：如果您的数据库更新频率更高，您应该调整 `ttl` 或删除缓存，以便查看者始终看到最新数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例表格），您的应用程序应该如下所示：

![应用程序截图](/images/databases/streamlit-app.png)
