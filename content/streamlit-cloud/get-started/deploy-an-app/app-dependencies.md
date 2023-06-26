---
标题: 应用程序依赖项
网址: /streamlit-community-cloud/get-started/deploy-an-app/app-dependencies
---

# 应用程序依赖项

应用程序无法正确构建的主要原因是Streamlit社区云找不到您的依赖项！所以请确保您：

1. 添加一个[requirements文件](#add-python-dependencies)来管理Python依赖项。
2. （可选）添加一个`packages.txt`文件来管理任何外部依赖项（即Python环境之外的Linux依赖项）。

<Note>

Python的依赖文件应该放在您的代码库的根目录或与您的Streamlit应用程序相同的目录中。

</Note>

### 添加Python依赖项

Streamlit会根据您的requirements文件的文件名来确定使用哪个Python依赖项管理器，按照以下顺序进行。Streamlit将停止并安装找到的第一个requirements文件。

| **文件名**           | **依赖管理器**     | **文档**                                                                                                                             |
| :----------------- | :----------------- | :---------------------------------------------------------------------------------------------------------------------------------- |
| `Pipfile`          | pipenv                 | **[文档](https://pipenv-fork.readthedocs.io/en/latest/basics.html)**                                                                  |
| `environment.yml`  | conda                  | **[文档](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-file-manually)** |
| `requirements.txt` | pip | **[文档](https://pip.pypa.io/en/stable/user_guide/#requirements-files)** |
| `pyproject.toml` | poetry | **[文档](https://python-poetry.org/docs/basic-usage/)** |

<注意>

在您的要求文件中只包括那些没有与标准Python一起分发的软件包。
安装。如果在要求文件中包含了基本Python模块中的任何一个，那么在尝试部署时将会出错。此外，我们建议您使用最新版本的Streamlit以确保完整的Streamlit社区云功能。请注意Streamlit的[当前要求](https://github.com/streamlit/streamlit/blob/develop/lib/setup.py)。
在规划环境时，请特别考虑软件包兼容性，尤其是`protobuf>=3.20,<5`。

</注意>

<警告>

您的应用程序应只使用一个要求文件。如果您包含多个文件（例如`requirements.txt`和`Pipfile`），Streamlit将首先在Streamlit应用程序的目录中查找；但是，如果未找到要求文件，则Streamlit将查找存储库的根目录。

</警告>

### apt-get 依赖项

如果在您的代码库根目录中存在 `packages.txt` 文件，我们将自动检测并解析它，并按照下面的描述安装列出的软件包。您可以在[官方文档](https://linux.die.net/man/8/apt-get)中了解更多关于 apt-get 的信息。

在 `packages.txt` 文件中逐行添加 **apt-get** 依赖的软件包名称。例如：

```bash
    freeglut3-dev
    libgtk2.0-dev
```
