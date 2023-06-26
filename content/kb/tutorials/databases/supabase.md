---
title: 将Streamlit连接到Supabase
slug: /knowledge-base/tutorials/databases/supabase
---

# 将Streamlit连接到Supabase

## 简介

本指南介绍了如何从Streamlit Community Cloud安全地访问Supabase实例。它使用[Supabase Python客户端库](https://github.com/supabase-community/supabase-py)和Streamlit的[secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。Supabase是开源的Firebase替代方案，基于PostgreSQL。

## 登录Supabase并创建一个项目

首先，访问[Supabase](https://app.supabase.io/)并使用您的GitHub账号注册一个免费账户。

<Flex>
<Image caption="使用GitHub登录" src="/images/databases/supabase-1.png" />
<Image caption="授权Supabase" src="/images/databases/supabase-2.png" />
</Flex>

登录后，您可以创建一个项目。

<Flex>
<Image caption="您的Supabase账户" src="/images/databases/supabase-3.png" />
<Image caption="创建一个新项目" src="/images/databases/supabase-4.png" />
</Flex>

创建项目后，您的屏幕应该如下图所示：

<Image src="/images/databases/supabase-5.png" />

<Important>

确保记下上述截图中突出显示的项目 API 密钥和项目 URL。☝️

您需要这些信息来从 Streamlit 连接到您的 Supabase 实例。

</Important>

## 创建一个 Supabase 数据库

现在您已经有一个项目了，可以创建一个数据库并填充一些示例数据。要做到这一点，点击同一项目页面上的**SQL编辑器**按钮，然后在SQL编辑器中点击**新建查询**按钮。

<Flex>
<Image caption="打开SQL编辑器" src="/images/databases/supabase-6.png" />
<Image caption="创建新查询" src="/images/databases/supabase-7.png" />
</Flex>

在SQL编辑器中，输入以下查询语句来创建一个数据库和一个带有一些示例值的表格：

```sql
CREATE TABLE mytable (
    name            varchar(80),
    pet             varchar(80)
);

INSERT INTO mytable VALUES ('Mary', 'dog'), ('John', 'cat'), ('Robert', 'bird');
```

点击**运行**以执行查询。要验证查询是否成功执行，请点击左侧菜单中的 **Table Editor** 按钮，然后选择您新创建的表 `mytable`。

<Flex>
<Image caption="编写并运行您的查询" src="/images/databases/supabase-8.png" />
<Image caption="在表编辑器中查看您的表" src="/images/databases/supabase-9.png" />
</Flex>

您已经创建了Supabase数据库，现在可以从Streamlit连接到它！

### 将Supabase项目URL和API密钥添加到本地应用程序的秘钥中

您的本地Streamlit应用程序将从位于应用程序根目录下的`.streamlit/secrets.toml`文件中读取秘钥。如果该文件不存在，请创建该文件，并在此处添加`supabase_url`和`supabase_key`。

```toml
# .streamlit/secrets.toml

supabase_url = "xxxx"
supabase_key = "xxxx"
```

将上面的`xxxx`替换为您在[第一步](/knowledge-base/tutorials/databases/supabase#sign-in-to-supabase-and-create-a-project)中从项目URL和API密钥中获得的信息。

<重要提示>

将此文件添加到`.gitignore`中，不要将其提交到您的GitHub仓库中！

</重要提示>

## 将您的应用程序密钥复制到云端

由于上面的 `secrets.toml` 文件没有提交到 GitHub，您需要单独将其内容传递给部署的应用程序（在Streamlit社区云上）。转到[应用程序仪表板](https://share.streamlit.io/)，在应用程序的下拉菜单中，点击**编辑密钥**。将 `secrets.toml` 的内容复制到文本区域中。有关更多信息，请参阅[密钥管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。

![Secrets manager screenshot](/images/databases/edit-secrets.png)

## 将 Supabase 添加到您的 requirements 文件中

将 [`supabase`](https://github.com/supabase-community/supabase-py) Python 客户端库添加到您的 `requirements.txt` 文件中，最好将其版本固定（将 `x.x.x` 替换为您想要安装的版本）：

```bash
# requirements.txt
supabase==x.x.x
```

## 编写Streamlit应用程序

将以下代码复制到您的Streamlit应用程序中并运行它。

```python
# streamlit_app.py

import streamlit as st
from supabase import create_client, Client

# Initialize connection.
# Uses st.cache_resource to only run once.
@st.cache_resource
def init_connection():
    url = st.secrets["supabase_url"]
    key = st.secrets["supabase_key"]
    return create_client(url, key)

supabase = init_connection()

# Perform query.
# Uses st.cache_data to only rerun when the query changes or after 10 min.
@st.cache_data(ttl=600)
def run_query():
    return supabase.table("mytable").select("*").execute()

rows = run_query()

# Print results.
for row in rows.data:
    st.write(f"{row['name']} has a :{row['pet']}:")

```

请参考上面的`st.cache_data`? 如果没有它，Streamlit将在每次应用程序重新运行时运行查询（例如在小部件交互时）。使用`st.cache_data`，它只在查询更改或10分钟后运行（这就是`ttl`的作用）。请注意：如果您的数据库更新得更频繁，您应该调整`ttl`或删除缓存，以便查看者始终看到最新的数据。在[Caching](/library/advanced-features/caching)中了解更多信息。

如果一切顺利（并且您使用了我们上面创建的示例表），您的应用程序应该如下图所示：

![应用程序截图](/images/databases/supabase-10.png)

由于Supabase在内部使用PostgresSQL，您还可以通过使用Supabase在“设置 > 数据库”下提供的连接字符串来连接到Supabase。从那里，您可以参考[PostgresSQL教程](/knowledge-base/tutorials/databases/postgresql)来连接您的数据库。
