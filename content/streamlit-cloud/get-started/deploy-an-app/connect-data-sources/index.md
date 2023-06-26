---
slug: /streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources
title: Connect to data sources
---

# 连接数据源

您的应用程序可能连接到某些数据源，确保连接安全非常重要。这些数据可能只是您在GitHub存储库中的一个CSV文件，但在许多情况下，它们可能是您通过API连接的私有数据源，在云服务上，或者可能是您公司的VPN。

Streamlit有一种主要的安全连接私有数据的方式:

- [密钥管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management): 安全存储像API密钥和TOML文件这样的密钥，然后可以作为环境变量在您的应用程序中访问。

我们还有一系列关于如何连接以下数据源的指南：

<DataSourcesContainer>
<DataSourcesCard href="/knowledge-base/tutorials/databases/aws-s3">

<Image pure alt="screenshot" src="/images/databases/s3.png" />

<h5>AWS S3</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/bigquery">

<Image pure alt="screenshot" src="/images/databases/bigquery.png" />

<h5>BigQuery</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/deta-base">

<Image pure alt="screenshot" src="/images/databases/deta-base.png" />

<h5>Deta Base</h5>

</DataSourcesCard>

<DataSourcesCard href="https://blog.streamlit.io/streamlit-firestore/">

<img pure alt="screenshot" src="/images/databases/firestore.png" />

<h5>Firestore (博客)</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/gcs">

<img pure alt="screenshot" src="/images/databases/gcs.png" />

<h5>Google Cloud Storage</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/mssql">

<img pure alt="screenshot" src="/images/databases/mssql.png" />

<h5>Microsoft SQL Server</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/mongodb">

<Image pure alt="screenshot" src="/images/databases/mongodb.png" />

<h5>MongoDB</h5>

</DataSourcesCard>

<DataSourcesCard href="/knowledge-base/tutorials/databases/mysql">

<Image pure alt="screenshot" src="/images/databases/mysql.png" />

<h5>MySQL</h5>

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

无法连接到数据？需要一种不同的安全连接方式吗？请在我们的[社区论坛](https://discuss.streamlit.io)上联系我们，以了解可行的选项！

</Note>