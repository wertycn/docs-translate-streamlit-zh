---
slug: /knowledge-base/dependencies/no-matching-distribution
title: ERROR No matching distribution found for
---

# 错误：找不到匹配的发行版

## 问题

在将应用程序部署到[Streamlit Community Cloud](https://streamlit.io/cloud)时，您收到了错误消息`ERROR: No matching distribution found for`。

## 解决方法

当您在Streamlit Community Cloud上部署应用程序时，在您的要求文件中存在以下一项或多项与[Python依赖项](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)相关的问题时，会出现此错误消息：

1. 该软件包是Python标准库的一部分。例如，如果您在要求文件中包含`base64`，并且它是Python标准库的一部分，您将看到**`ERROR: No matching distribution found for base64`**。解决方法是不在要求文件中包含该软件包。只在要求文件中包含那些没有随标准Python安装一起分发的软件包。
2. 在您的要求文件中，软件包名称拼写错误。在将其包含在要求文件中之前，请仔细检查软件包名称。
3. 该软件包不支持您的Streamlit应用程序所运行的操作系统。例如，当您部署到Streamlit社区云时，您会看到**`ERROR: No matching distribution found for pywin32`**。`pywin32`模块提供了从Python访问许多Windows API的功能。在Streamlit社区云中执行的应用程序在Linux环境中运行。因此，`pywin32`无法在非Windows系统上安装，包括在Streamlit社区云上。解决方案是要么从您的需求文件中排除`pywin32`，要么将您的应用程序部署在提供Windows机器的云服务上。

相关论坛帖子：

- https://discuss.streamlit.io/t/error-no-matching-distribution-found-for-base64/15758
- https://discuss.streamlit.io/t/error-could-not-find-a-version-that-satisfies-the-requirement-pywin32-301-from-versions-none/15343/2