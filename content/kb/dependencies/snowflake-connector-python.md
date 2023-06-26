---
title: 在Streamlit Community Cloud上安装Python的Snowflake连接器
slug: /knowledge-base/dependencies/snowflake-connector-python-streamlit-cloud
---

# 在Streamlit Community Cloud上安装Python的Snowflake连接器

Snowflake的Python连接器可以在[PyPI](https://pypi.org/project/snowflake-connector-python/)上找到，并且安装指南可以在[Snowflake文档](https://docs.snowflake.com/en/user-guide/python-connector-install.html#step-1-install-the-connector)中找到。在安装连接器时，Snowflake建议安装特定版本的依赖库。以下步骤将帮助您在Streamlit Community Cloud上安装连接器及其依赖项：

1. 确定要安装的Snowflake Connector for Python的版本。
2. 确定要在Streamlit Community Cloud上使用的Python版本。
3. 要安装连接器和相关的库，请选择该版本连接器和Python的[requirements文件](https://github.com/snowflakedb/snowflake-connector-python/tree/main/tested_requirements)。
4. 将要求文件的原始GitHub URL添加到您的`requirements.txt`文件中，并在行前添加`-r`。
   例如，如果您想在Python 3.9上安装`2.7.9`版本的连接器，请将以下行添加到您的`requirements.txt`文件中：

   ```bash
   -r https://raw.githubusercontent.com/snowflakedb/snowflake-connector-python/v2.7.9/tested_requirements/requirements_39.reqs
   ```

5. 在Streamlit Community Cloud上，在部署应用程序之前，通过点击"高级设置"来选择适合您的应用程序的Python版本:
<div style={{ maxWidth: '65%', marginBottom: '-3em', marginLeft: '6em', marginTop: '-2em' }}>
    <Image src="/images/streamlit-community-cloud/advanced-settings.png" />
</div>

就是这样！您已经可以在Streamlit Community Cloud上使用Snowflake Connector for Python了。❄️🎈

<Tip>

由于Snowflake的依赖关系要求文件（`.reqs`）包含了连接器的固定版本，因此**不需要**在requirements.txt中单独添加连接器的条目。

</Tip>

其他资源：

- [安装Python连接器](https://docs.snowflake.com/en/user-guide/python-connector-install.html#step-1-install-the-connector)
- [无法使用snowflake-connector-python部署streamlit应用](https://discuss.streamlit.io/t/unable-to-deploy-streamlit-app-with-snowflake-connector-python/27318)
- [Snowflake Connector的先决条件Python包](https://docs.snowflake.com/en/user-guide/python-connector-install.html#label-python-connector-prerequisites-python-packages)
