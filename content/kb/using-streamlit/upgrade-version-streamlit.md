---
slug: /knowledge-base/using-streamlit/how-upgrade-latest-version-streamlit
title: How do I upgrade to the latest version of Streamlit?
---

# 如何升级到最新版本的Streamlit？

要升级到最新版本的Streamlit，您可以按照以下步骤操作：

1. 打开终端或命令提示符。
2. 运行以下命令来升级Streamlit的Python包：
   ```
   pip install --upgrade streamlit
   ```
3. 等待安装过程完成。
4. 检查Streamlit的版本是否已成功升级：
   ```
   streamlit --version
   ```
   如果显示了最新的版本号，则表示升级成功。

请注意，升级Streamlit可能会导致一些API或功能的变化，请确保您的应用程序在升级后仍能正常工作，并根据需要进行适当的更改和调整。

我们建议您升级到最新的Streamlit官方发布版本，以便获得最新的、尖端的功能。如果您尚未安装Streamlit，请阅读我们的[安装指南](/library/get-started/installation)。它将帮助您设置虚拟环境，并指导您在Windows、macOS和Linux上安装Streamlit。无论您使用哪个软件包管理工具和操作系统，我们建议您在虚拟环境中运行本页面上的命令。

如果您之前已经安装了Streamlit并且想要升级到最新版本，根据您的依赖管理器，可以按照以下步骤进行操作。

## Pipenv

Streamlit在macOS和Linux上的官方环境管理器是[Pipenv](https://pypi.org/project/pipenv/)。

1. 导航到包含您的Pipenv环境的项目文件夹：

```bash
cd myproject
```

2. 激活该环境，升级Streamlit，并验证您是否拥有最新版本：

```bash
pipenv shell
pip install --upgrade streamlit
streamlit version
```

如果您想使用一个易于复制的环境，每次安装或更新包时，请将 `pip` 替换为 `pipenv` ：

```bash
pipenv update streamlit
pipenv run streamlit version
```

## Conda

1. 激活安装了Streamlit的conda环境：

```bash
conda activate $ENVIRONMENT_NAME
```

请确保将`$ENVIRONMENT_NAME` ☝️替换为您的conda环境的名称！

2. 在活动的conda环境中更新Streamlit，并验证您是否有最新版本：

```bash
conda update -c conda-forge streamlit -y
streamlit version
```

## 诗歌

为了通过[Poetry](https://python-poetry.org/)获取最新版本的Streamlit，并验证您是否拥有最新版本，请运行：

```bash
poetry update streamlit
streamlit version
```