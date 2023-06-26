---
title: 在Streamlit社区云上升级您的应用程序的Streamlit版本
slug: /knowledge-base/deploy/upgrade-streamlit-version-on-streamlit-cloud
---

# 在Streamlit社区云上升级您的应用程序的Streamlit版本

如果您想使用Streamlit社区云上的一个新功能，但您的应用程序正在运行旧版本的Streamlit库，不用担心！您只需升级应用程序的Streamlit版本即可。以下是五种升级方式，根据您的[应用程序管理依赖项的方式](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies)而定：

## 没有依赖文件

当存储库中没有依赖文件时，应用程序将始终使用在将应用程序首次部署到Streamlit社区云时存在的相同Streamlit版本；即使您重新启动应用程序！换句话说，Streamlit社区云会自动为您固定版本，以便在将来重新启动时应用程序不会突然中断。

如果您的应用程序依赖于特定版本的Streamlit，您可能希望避免进入这种情况。这就是为什么我们鼓励您使用依赖文件，并固定您所需的Streamlit版本。我们将在下面的章节中详细介绍这一点。

## `requirements.txt`

1. 如果Streamlit版本未固定（即，要求文件中只包含一行`streamlit`而没有其他内容）：
   - 如上所述，重启应用程序。
2. 如果Streamlit版本被固定（例如，`streamlit==1.4.0`）：
   - 在requirements文件中调整固定版本，并将其推送到GitHub。
   - Streamlit Community Cloud上的应用程序将在检测到这些更改后自动重新启动。

## pipenv/poetry/conda

如果您使用这些依赖管理工具之一，您可能知道需要做什么。😉

1. 在本地计算机上运行以下命令以更新Streamlit包：
   - `pipenv update streamlit` 或者
   - 运行`poetry update streamlit`或
  - 使用已激活的conda环境运行:
    - `pip install -U streamlit` **和**
    - `conda env export`
2. 然后将任何更改推送到GitHub的Pipfile.lock或poetry.lock或environment.yml。
3. 只要检测到这些更改，Streamlit Community Cloud上的应用程序将自动重新启动。
