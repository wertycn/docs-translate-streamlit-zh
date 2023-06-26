---
slug: /knowledge-base/tutorials/databases/public-gsheet
title: Connect Streamlit to a public Google Sheet
---

# 将 Streamlit 连接到公共 Google Sheet

## 简介

本指南说明如何从 Streamlit Community Cloud 安全地访问公共 Google Sheet。它使用 [pandas](https://pandas.pydata.org/) 库和 Streamlit 的 [secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management) 功能。

这种方法需要您启用Google Sheet的链接共享。虽然共享链接不会出现在您的代码中（实际上起到了密码的作用！），但知道链接的人可以获取Sheet中的所有数据。如果您不希望这样，可以按照（更复杂的）指南[将Streamlit连接到私有Google Sheet](private-gsheet)。

## 创建一个Google Sheet并打开链接共享

<注意>

如果您已经拥有要访问的Sheet，请随时[跳至下一步]
[步骤](#add-the-sheets-url-to-your-local-app-secrets)。

</注意>

<Flex>
<Image alt="screenshot 1" src="/images/databases/public-gsheet-1.png" />
<Image alt="screenshot 1" src="/images/databases/public-gsheet-2.png" />
</Flex>

## 将 Sheets URL 添加到本地应用程序的 secrets

您的本地 Streamlit 应用程序将从应用程序根目录中的 `.streamlit/secrets.toml` 文件中读取密钥。如果该文件尚不存在，请创建该文件，并将您的 Google Sheet 的共享链接添加到其中，如下所示：

```toml
# .streamlit/secrets.toml

public_gsheets_url = "https://docs.google.com/spreadsheets/d/xxxxxxx/edit#gid=0"
```

<重要>

将此文件添加到`.gitignore`中，不要将其提交到您的GitHub存储库中！

</重要>

## 将您的应用程序密钥复制到云端

由于上面的 `secrets.toml` 文件没有提交到 GitHub，您需要将其内容单独传递给部署的应用程序（在 Streamlit Community Cloud 上）。转到[应用仪表板](https://share.streamlit.io/)，在应用的下拉菜单中，点击 **编辑 Secrets**。将 `secrets.toml` 的内容复制到文本区域中。更多信息请参阅[Secrets 管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 编写你的Streamlit应用程序

将下面的代码复制到你的Streamlit应用程序中并运行它。

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

查看上面的`st.cache_data`? 如果没有它，Streamlit将在每次应用重新运行时运行查询（例如，小部件交互）。使用`st.cache_data`，它只会在查询更改或经过10分钟后运行（这就是`ttl`的作用）。请注意：如果您的数据库更新频率更高，您应该调整`ttl`或删除缓存，以便观看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例表），您的应用程序应该是这个样子的：

![应用程序截图](/images/databases/streamlit-app.png)