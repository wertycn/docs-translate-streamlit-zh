---
标题：连接数据源
slug：/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources
---

# 连接数据源

您的应用程序可能连接到某些数据源，确保连接是安全的非常重要。这些数据可能只是您在GitHub仓库中的一个csv文件，但在许多情况下，它们可能是您通过API连接的私有数据源，位于云服务上，或者可能位于您公司的VPN中。

Streamlit有一种主要的方式可以安全地连接私有数据：

- [Secrets管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)：安全地存储像API密钥和TOML文件这样的秘密，然后在您的应用程序中作为环境变量访问。

我们还有一系列关于如何连接到以下内容的指南：

<DataSourcesContainer>
<DataSourcesCard href="/knowledge-base/tutorials/databases/aws-s3">

![screenshot](/images/databases/s3.png)

#### AWS S3

</DataSourcesCard>

</DataSourcesCard>

![screenshot](/images/databases/bigquery.png)

#### BigQuery

</DataSourcesCard>

</DataSourcesCard>

![screenshot](/images/databases/deta-base.png)

#### Deta Base

</DataSourcesCard>

<DataSourcesCard href="https://blog.streamlit.io/streamlit-firestore/">

<Image pure alt="screenshot" src="/images/databases/firestore.png" />

<h5>Firestore (博客)</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/gcs">

<Image pure alt="screenshot" src="/images/databases/gcs.png" />

<h5>Google Cloud Storage</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/mssql">

![screenshot](/images/databases/mssql.png)

## Microsoft SQL Server

</DataSourcesCard>

</DataSourcesCard>

![screenshot](/images/databases/mongodb.png)

## MongoDB

</DataSourcesCard>

</DataSourcesCard>

![screenshot](/images/databases/mysql.png)

## MySQL

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/postgresql">

<Image pure alt="screenshot" src="/images/databases/postgresql.png" />

<h5>PostgreSQL</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/private-gsheet">

<Image pure alt="screenshot" src="/images/databases/gsheet.png" />

<h5>私有Google表格</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/public-gsheet">

<Image pure alt="screenshot" src="/images/databases/gsheet.png" />

<h5>公共Google表格</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/snowflake">

<Image pure alt="screenshot" src="/images/databases/snowflake.png" />

<h5>Snowflake</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/supabase">

<Image pure alt="screenshot" src="/images/databases/supabase.png" />

<h5>Supabase</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/tableau">

<Image pure alt="screenshot" src="/images/databases/tableau.png" />

<h5>Tableau</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/tidb">

<Image pure alt="screenshot" src="/images/databases/tidb.png" />

<h5>TiDB</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/tigergraph">

<Image pure alt="screenshot" src="/images/databases/tigergraph.png" />

<h5>TigerGraph</h5>

</DataSourcesCard>
</DataSourcesContainer>

<Note>

无法连接数据？需要其他安全连接方式？请在我们的[社区论坛](https://discuss.streamlit.io)上咨询以了解更多选项！

</Note>
