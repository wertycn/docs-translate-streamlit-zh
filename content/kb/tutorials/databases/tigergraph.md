---
title: 将Streamlit连接到TigerGraph
slug: /knowledge-base/tutorials/databases/tigergraph
---

# 将Streamlit连接到TigerGraph

## 简介

本指南介绍了如何在Streamlit Community Cloud中安全地访问TigerGraph数据库。它使用[pyTigerGraph](https://pytigergraph.github.io/pyTigerGraph/GettingStarted/)库和Streamlit的[secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)功能。

## 创建一个TigerGraph云数据库

首先，按照官方教程在TigerGraph Cloud中创建一个TigerGraph实例，可以选择[博客](https://www.tigergraph.com/blog/getting-started-with-tigergraph-3-0/)或[视频](https://www.youtube.com/watch?v=NtNW2e8MfCQ)进行操作。记下您的用户名、密码和子域名。

在本教程中，我们将使用COVID-19起始套件。在设置解决方案时，请选择“COVID-19分析”选项。

![TG_Cloud_COVID19](/images/databases/tigergraph-1.png)

启动后，请确保已下载数据并安装查询。

![TG_Cloud_Schema](/images/databases/tigergraph-2.png)

## 将用户名和密码添加到本地应用程序密钥中

您的本地Streamlit应用将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取密钥。如果该文件尚不存在，请创建该文件，并按照下面的示例添加您的TigerGraph Cloud实例用户名、密码、图名称和子域名。

```toml
# .streamlit/secrets.toml

[tigergraph]
host = "https://xxx.i.tgcloud.io/"
username = "xxx"
password = "xxx"
graphname = "xxx"
```

<重要>

将此文件添加到`.gitignore`中，并不要将其提交到您的 GitHub 仓库！

</重要>

## 将应用程序密钥复制到云端

由于上面的`secrets.toml`文件没有提交到GitHub上，您需要单独将其内容传递给部署的应用程序（在Streamlit Community Cloud上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，单击“编辑Secrets”。将`secrets.toml`的内容复制到文本区域中。有关更多信息，请访问[Secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)页面。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将 PyTigerGraph 添加到您的 requirements 文件

将 pyTigerGraph 包添加到您的 `requirements.txt` 文件中，最好固定其版本（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
pyTigerGraph==x.x.x
```

## 编写您的Streamlit应用程序

将下面的代码复制到您的Streamlit应用程序中并运行。确保调整您的图表和查询的名称。

```python
# streamlit_app.py

import streamlit as st
import pyTigerGraph as tg

# Initialize connection.
conn = tg.TigerGraphConnection(**st.secrets["tigergraph"])
conn.apiToken = conn.getToken(conn.createSecret())

# Pull data from the graph by running the "mostDirectInfections" query.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def get_data():
    most_infections = conn.runInstalledQuery("mostDirectInfections")[0]["Answer"][0]
    return most_infections["v_id"], most_infections["attributes"]

items = get_data()

# Print results.
st.title(f"Patient {items[0]} has the most direct infections")
for key, val in items[1].items():
    st.write(f"Patient {items[0]}'s {key} is {val}.")
```

请查看上面的`st.cache_data`? 如果没有它，Streamlit将在应用程序重新运行时（例如在小部件交互时）每次运行查询。使用`st.cache_data`，它只在查询更改或10分钟后运行（这就是`ttl`的作用）。请注意：如果您的数据库更新频率更高，您应该调整`ttl`或删除缓存，以便查看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例数据），您的应用程序应该如下图所示：

![Final_App](/images/databases/tigergraph-3.png)
