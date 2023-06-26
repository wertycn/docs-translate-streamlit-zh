---
slug: /knowledge-base/tutorials/databases/tigergraph
title: Connect Streamlit to TigerGraph
---

# 连接Streamlit到TigerGraph

## 简介

本指南将解释如何从Streamlit Community Cloud安全访问TigerGraph数据库。它使用了[pyTigerGraph](https://pytigergraph.github.io/pyTigerGraph/GettingStarted/)库和Streamlit的[secrets management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)功能。

## 创建TigerGraph云数据库

首先，请按照官方教程在TigerGraph Cloud中创建一个TigerGraph实例，可以参考[博客](https://www.tigergraph.com/blog/getting-started-with-tigergraph-3-0/)或者[视频](https://www.youtube.com/watch?v=NtNW2e8MfCQ)。记下您的用户名、密码和子域名。

在本教程中，我们将使用COVID-19起始套件。在设置解决方案时，请选择“COVID-19分析”选项。

![TG_Cloud_COVID19](/images/databases/tigergraph-1.png)

一旦启动，请确保已下载数据并安装了查询。

![TG_Cloud_Schema](/images/databases/tigergraph-2.png)

## 在本地应用程序密钥中添加用户名和密码

您的本地Streamlit应用程序将从应用程序根目录中的`.streamlit/secrets.toml`文件中读取密钥。如果该文件尚不存在，请创建该文件，并按照下面的示例添加您的TigerGraph Cloud实例用户名、密码、图名称和子域名。

```toml
# .streamlit/secrets.toml

[tigergraph]
host = "https://xxx.i.tgcloud.io/"
username = "xxx"
password = "xxx"
graphname = "xxx"
```

<重要>

将此文件添加到`.gitignore`中，并不要将其提交到您的GitHub仓库！

</重要>

## 将您的应用程序密钥复制到云端

由于上述的`secrets.toml`文件未提交到GitHub，您需要单独将其内容传递给部署的应用程序（在Streamlit Community Cloud上）。请前往[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击"编辑 Secrets"。将`secrets.toml`的内容复制到文本区域中。更多信息请参阅[Secrets Management](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将PyTigerGraph添加到您的requirements文件中

将pyTigerGraph包添加到您的`requirements.txt`文件中，最好指定其版本（将`x.x.x`替换为您想要安装的版本）：

```bash
# requirements.txt
pyTigerGraph==x.x.x
```

## 编写您的Streamlit应用程序

将以下代码复制到您的Streamlit应用程序中并运行。确保调整您的图表和查询的名称。

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

请参考上面的 `st.cache_data`。如果没有它，Streamlit将在每次应用重新运行时运行查询（例如在小部件交互时）。使用 `st.cache_data`，它只在查询更改或超过10分钟后运行（这就是 `ttl` 的作用）。注意：如果您的数据库更新频率更高，您应该调整 `ttl` 或删除缓存，以便观众始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了上面创建的示例数据），您的应用程序应该如下图所示：

![最终应用程序](/images/databases/tigergraph-3.png)