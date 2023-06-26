---
slug: /knowledge-base/dependencies/install-package-not-pypi-conda-available-github
title: How to install a package not on PyPI/Conda but available on GitHub
---

# 如何安装不在PyPI/Conda上但在GitHub上可用的软件包

## 概述

您是否正在尝试将应用程序部署到[Streamlit Community Cloud](/streamlit-community-cloud)，但不知道如何在要求文件中指定一个在公共GitHub仓库上可用但不在PyPI或Conda等软件包索引中的Python依赖项？如果是这样，请继续阅读以了解如何操作！

假设您想要从GitHub安装`SomePackage`及其Python依赖项，GitHub是一个流行的版本控制系统（VCS）Git的托管服务。假设`SomePackage`可以在以下URL找到：`https://github.com/SomePackage.git`。

pip（通过`requirements.txt`）[支持](https://pip.pypa.io/en/stable/topics/vcs-support/)从GitHub安装。这种支持需要一个可用的可执行文件（用于Git）。它通过URL前缀`git+`来使用。

## 指定 GitHub 网址

要安装 `SomePackage`，请在您的 `requirements.txt` 文件中包含以下内容：

```bash
git+https://github.com/SomePackage#egg=SomePackage
```

您甚至可以指定一个"git ref"，比如分支名称、提交哈希值或者标签名称，如下面的示例所示。

## 指定一个Git分支名称

通过在`requirements.txt`中指定一个分支名称，如`main`、`master`、`develop`等，来安装`SomePackage`：

```bash
git+https://github.com/SomePackage.git@main#egg=SomePackage
```

## 指定提交哈希值

在`requirements.txt`中指定一个提交哈希值来安装`SomePackage`：

```bash
git+https://github.com/SomePackage.git@eb40b4ff6f7c5c1e4366cgfg0671291bge918#egg=SomePackage
```

## 指定标签

在 `requirements.txt` 文件中通过指定标签安装 `SomePackage` 包：

```bash
git+https://github.com/SomePackage.git@v1.1.0#egg=SomePackage
```

## 限制

目前**无法**使用URI形式从私有GitHub仓库安装私有包。

```bash
git+https://{token}@github.com/user/project.git@{version}
```

`version` 是一个标签、分支或者提交记录。而 `token` 是一个具有只读权限的个人访问令牌。Streamlit Community Cloud 仅支持从公共 GitHub 仓库安装公共包。