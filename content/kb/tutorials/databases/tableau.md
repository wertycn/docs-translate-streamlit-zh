---
slug: /knowledge-base/tutorials/databases/tableau
title: Connect Streamlit to Tableau
---

# 将Streamlit连接到Tableau

## 简介

本指南介绍了如何从Streamlit Community Cloud安全地访问Tableau上的数据。它使用[tableauserverclient](https://tableau.github.io/server-client-python/#)库和Streamlit的[secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

## 创建Tableau站点

<Note>

如果您已经有一个要使用的数据库，可以随意使用
要[跳转到下一步](#create-personal-access-tokens)。

</Note>

为了简单起见，我们在这里使用的是Tableau的云版本，但是这个指南同样适用于自托管部署。首先，注册[Tableau Online](https://www.tableau.com/products/cloud-bi)或登录。创建一个工作簿或者运行"Dashboard Starters"下的示例工作簿。

![Tableau截图1](/images/databases/tableau-1.png)

## 创建个人访问令牌

虽然Tableau API允许通过用户名和密码进行身份验证，但在生产应用程序中，您应该使用[个人访问令牌](https://help.tableau.com/current/server/en-us/security_personal_access_tokens.htm)。

转到您的[Tableau Online主页](https://online.tableau.com/)，创建一个访问令牌，并记录下令牌名称和密钥。

<Flex>
<Image alt="Tableau截图2" src="/images/databases/tableau-2.png" />
<Image alt="Tableau截图3" src="/images/databases/tableau-3.png" />
</Flex>

<Note>

如果个人访问令牌在连续15天内未使用，则会过期。

</Note>

## 将令牌添加到本地应用程序密钥

您的本地Streamlit应用程序将从应用程序根目录中的文件`.streamlit/secrets.toml`中读取密钥。如果该文件尚不存在，请创建该文件，并按照以下方式添加您的令牌、在设置期间创建的站点名称以及您的Tableau服务器的URL:

```toml
# .streamlit/secrets.toml

[tableau]
token_name = "xxx"
token_secret = "xxx"
server_url = "https://abc01.online.tableau.com/"
site_id = "streamlitexample"  # in your site's URL behind the server_url
```

<重要>

将此文件添加到`.gitignore`中，不要将其提交到您的GitHub仓库！

</重要>

## 将应用程序的机密信息复制到云端

由于上面的 `secrets.toml` 文件没有提交到 GitHub，您需要单独将其内容传递给已部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击 **编辑 Secrets**。将 `secrets.toml` 的内容复制到文本区域中。有关更多信息，请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager截图](/images/databases/edit-secrets.png)

## 将tableauserverclient添加到您的requirements文件中

将[tableauserverclient](https://tableau.github.io/server-client-python/#)包添加到您的`requirements.txt`文件中，最好固定其版本（将`x.x.x`替换为您想要安装的版本）。

```bash
# requirements.txt
tableauserverclient==x.x.x
```

## 编写Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行。请注意，此代码只展示了一些您可以获取的数据选项 - 探索[tableauserverclient](https://tableau.github.io/server-client-python/#)库以获取更多信息！

```python
# streamlit_app.py

import streamlit as st
import tableauserverclient as TSC


# Set up connection.
tableau_auth = TSC.PersonalAccessTokenAuth(
    st.secrets["tableau"]["token_name"],
    st.secrets["tableau"]["personal_access_token"],
    st.secrets["tableau"]["site_id"],
)
server = TSC.Server(st.secrets["tableau"]["server_url"], use_server_version=True)


# Get various data.
# Explore the tableauserverclient library for more options.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def run_query():
    with server.auth.sign_in(tableau_auth):

        # Get all workbooks.
        workbooks, pagination_item = server.workbooks.get()
        workbooks_names = [w.name for w in workbooks]

        # Get views for first workbook.
        server.workbooks.populate_views(workbooks[0])
        views_names = [v.name for v in workbooks[0].views]

        # Get image & CSV for first view of first workbook.
        view_item = workbooks[0].views[0]
        server.views.populate_image(view_item)
        server.views.populate_csv(view_item)
        view_name = view_item.name
        view_image = view_item.image
        # `view_item.csv` is a list of binary objects, convert to str.
        view_csv = b"".join(view_item.csv).decode("utf-8")

        return workbooks_names, views_names, view_name, view_image, view_csv

workbooks_names, views_names, view_name, view_image, view_csv = run_query()


# Print results.
st.subheader("📓 Workbooks")
st.write("Found the following workbooks:", ", ".join(workbooks_names))

st.subheader("👁️ Views")
st.write(
    f"Workbook *{workbooks_names[0]}* has the following views:",
    ", ".join(views_names),
)

st.subheader("🖼️ Image")
st.write(f"Here's what view *{view_name}* looks like:")
st.image(view_image, width=300)

st.subheader("📊 Data")
st.write(f"And here's the data for view *{view_name}*:")
st.write(pd.read_csv(StringIO(view_csv)))
```

请查看上面的 `st.cache_data`？如果没有它，Streamlit将在每次应用重新运行时运行查询（例如在小部件交互时）。使用 `st.cache_data`，它仅在查询更改或经过10分钟后才运行（这就是 `ttl` 的作用）。注意：如果您的数据库更新频率更高，您应该调整 `ttl` 或者移除缓存，以便查看者始终看到最新的数据。在 [Caching](/library/advanced-features/caching) 中了解更多信息。

如果一切顺利，您的应用程序应该如下所示（根据您的工作簿可能会有所不同）：

![Tableau截图4](/images/databases/tableau-4.png)