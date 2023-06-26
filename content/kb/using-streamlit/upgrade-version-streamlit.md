---
title: 如何升级到最新版本的Streamlit？
slug: /knowledge-base/using-streamlit/how-upgrade-latest-version-streamlit
---

# 如何升级到最新版本的Streamlit？

我们建议您升级到最新的Streamlit官方版本，以便使用最新的、尖端的功能。如果您还没有安装Streamlit，请阅读我们的[安装指南](/library/get-started/installation)。它将帮助您设置虚拟环境，并指导您在Windows、macOS和Linux上安装Streamlit。无论您使用哪种软件包管理工具和操作系统，我们建议在虚拟环境中运行本页面上的命令。

如果您之前已经安装了Streamlit并且想要升级到最新版本，根据您使用的依赖管理工具，可以按照以下步骤进行操作。

## Pipenv

Streamlit在macOS和Linux上的官方支持的环境管理工具是[Pipenv](https://pypi.org/project/pipenv/)。

1. 进入包含Pipenv环境的项目文件夹：

```bash
cd myproject
```

2. 激活该环境，升级Streamlit，并验证您是否拥有最新版本：

```bash
pipenv shell
pip install --upgrade streamlit
streamlit version
```

或者，如果您想使用一个易于复现的环境，在每次安装或更新包时，将`pip`替换为`pipenv`：

```bash
pipenv update streamlit
pipenv run streamlit version
```

## Conda

1. 激活安装了Streamlit的conda环境：

```bash
conda activate $ENVIRONMENT_NAME
```

请确保使用您的conda环境的名称替换`$ENVIRONMENT_NAME` ☝️！

2. 在活动的conda环境中更新Streamlit并验证您是否具有最新版本：

```bash
conda update -c conda-forge streamlit -y
streamlit version
```

## 诗歌

为了使用 [Poetry](https://python-poetry.org/) 获取最新版本的Streamlit，并验证您是否有最新版本，请运行：

```bash
poetry update streamlit
streamlit version
```
