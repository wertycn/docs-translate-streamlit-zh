---
title: 错误 找不到匹配的分发包
slug: /knowledge-base/dependencies/no-matching-distribution
---

# 错误: 找不到匹配的分发包

## 问题

当您在[Streamlit Community Cloud](https://streamlit.io/cloud)部署应用程序时，会收到错误消息`错误: 找不到匹配的分发包`。

## 解决方案

在将应用程序部署到Streamlit Community Cloud时，如果您的要求文件中存在以下问题之一，将会出现此错误与您的[Python依赖项](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)相关：

1.该软件包是[Python标准库](https://docs.python.org/3/py-modindex.html)的一部分。例如，如果您在要求文件中包含[`base64`](https://docs.python.org/3/library/base64.html)，您将看到**`ERROR: No matching distribution found for base64`**。这是因为该软件包是Python标准库的一部分。解决方法是不在您的要求文件中包含该软件包。只在您的要求文件中包含那些不随标准Python安装分发的软件包。
2. 在您的 requirements 文件中，软件包名称拼写错误。在将其包含在您的 requirements 文件之前，请仔细检查软件包名称。
3. 该软件包不支持您的Streamlit应用程序所在的操作系统。例如，当部署到Streamlit社区云时，您会看到**`ERROR: No matching distribution found for pywin32`**错误。`pywin32`模块允许从Python访问许多Windows API。部署到Streamlit社区云的应用程序在Linux环境中执行。因此，`pywin32`无法在非Windows系统上安装，包括在Streamlit社区云上。解决方案是要么在您的要求文件中排除`pywin32`，要么将应用程序部署到提供Windows机器的云服务上。

相关论坛帖子：

- https://discuss.streamlit.io/t/error-no-matching-distribution-found-for-base64/15758
- https://discuss.streamlit.io/t/error-could-not-find-a-version-that-satisfies-the-requirement-pywin32-301-from-versions-none/15343/2
