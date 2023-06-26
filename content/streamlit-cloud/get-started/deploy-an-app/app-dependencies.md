---
slug: /streamlit-community-cloud/get-started/deploy-an-app/app-dependencies
title: App dependencies
---

# 应用程序依赖项

应用程序无法正确构建的主要原因是Streamlit Community Cloud无法找到您的依赖项！因此，请确保您：

1. 添加一个用于Python依赖项的[requirements文件](#add-python-dependencies)。
2. （可选）添加一个`packages.txt`文件来管理任何外部依赖项（即Python环境之外的Linux依赖项）。

<注意>

Python的requirements文件应放置在您的代码库的根目录或与主应用程序文件相同的目录中。
将目录设置为您的Streamlit应用程序。

</Note>

### 添加Python依赖项

Streamlit根据您的requirements文件的文件名确定使用的Python依赖项管理器，按照以下顺序。Streamlit将停止并安装找到的第一个requirements文件。

| **文件名**       | **依赖项管理器** | **文档**                                                                                                                     |
| :----------------- | :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------ |
| `Pipfile`          | pipenv                 | **[文档](https://pipenv-fork.readthedocs.io/en/latest/basics.html)**                                                                  |
| `environment.yml`  | conda                  | **[文档](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-file-manually)** |
| `requirements.txt` | pip                    | **[文档](https://pip.pypa.io/en/stable/user_guide/#requirements-files)**                                                              |
| `pyproject.toml`   | poetry                 | **[文档](https://python-poetry.org/docs/basic-usage/)**                                                                               |

<注意>

在您的需求文件中只包含非标准Python安装中未分发的软件包。如果[来自基本Python的任何模块](https://docs.python.org/3/py-modindex.html)
如果在要求文件中包含了无法满足的依赖项，尝试部署时会出现错误。此外，我们建议您使用最新版本的Streamlit，以确保完整的Streamlit Community Cloud功能。在计划环境时，请务必注意Streamlit的[当前要求](https://github.com/streamlit/streamlit/blob/develop/lib/setup.py)，特别是`protobuf>=3.20,<5`的包兼容性。

</注意>

<警告>

您的应用程序只应使用一个要求文件。如果您包含多个文件（例如 `requirements.txt` 和 `Pipfile`），Streamlit 将首先在 Streamlit 应用程序的目录中查找；但是，如果没有找到要求文件，Streamlit 将会在存储库的根目录查找。

</Warning>

### apt-get 依赖项

如果在您的存储库的根目录中存在`packages.txt`文件，我们将自动检测、解析并安装其中列出的软件包，如下所述。您可以在它们的[文档](https://linux.die.net/man/8/apt-get)中了解更多关于apt-get的信息。

将**apt-get**依赖项添加到`packages.txt`中，每行一个软件包名称。例如：

```bash
    freeglut3-dev
    libgtk2.0-dev
```