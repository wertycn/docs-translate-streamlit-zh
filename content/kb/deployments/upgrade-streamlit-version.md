---
slug: /knowledge-base/deploy/upgrade-streamlit-version-on-streamlit-cloud
title: Upgrade the Streamlit version of your app on Streamlit Community Cloud
---

# 在Streamlit Community Cloud上升级应用程序的Streamlit版本

想要使用Streamlit的新功能，但你在Streamlit Community Cloud上运行的应用程序使用的是旧版本的Streamlit库？如果是这样的话，不用担心！你只需要升级应用程序的Streamlit版本即可。以下是五种升级方式，根据你的[应用程序管理依赖项的方式](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies)来选择：

## 没有依赖文件

当存储库中没有依赖文件时，应用程序将始终使用在应用程序**首次**部署在Streamlit社区云上时存在的相同版本的Streamlit；即使您重新启动应用程序！换句话说，Streamlit社区云会自动为您固定版本，以防止应用程序在将来重新启动时突然中断。

如果您的应用程序依赖于特定版本的Streamlit，您可能希望避免陷入这种情况。这就是为什么我们鼓励您使用依赖文件并固定所需的Streamlit版本。我们将在下面的章节中详细介绍这个问题。

## `requirements.txt`

1. 如果Streamlit版本没有固定（即要求文件中只包含一行`streamlit`）：
   - 请按照上面的描述重新启动应用程序。
2. 如果 Streamlit 的版本被固定（例如 `streamlit==1.4.0`）：
   - 在 requirements 文件中适应固定的版本，并将其推送到 GitHub。
   - 只要 Streamlit Community Cloud 检测到这些更改，应用程序将自动重新启动。

## pipenv/poetry/conda

如果您使用这些依赖管理工具之一，您可能知道您需要做什么。😉

1. 在本地计算机上运行以下命令以更新 Streamlit 包：
   - `pipenv update streamlit` 或者
   - 运行`poetry update streamlit`或者
   - 在激活的conda环境中运行：
      - `pip install -U streamlit` **和**
      - `conda env export`
2. 然后将任何更改推送到GitHub上的Pipfile.lock或poetry.lock或environment.yml文件。
3. 当Streamlit Community Cloud检测到这些更改时，应用程序将自动重新启动。