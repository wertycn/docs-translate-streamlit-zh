---
slug: /library/get-started/installation
title: Installation
---

# 安装 Streamlit

## 目录

1. [先决条件](#先决条件)
2. [在 Windows 上安装 Streamlit](#在-windows-上安装-streamlit)
3. [在 macOS/Linux 上安装 Streamlit](#在-macoslinux-上安装-streamlit)

## 先决条件

在开始之前，您需要准备一些东西：

- 您喜欢的集成开发环境（IDE）或文本编辑器
- [Python 3.7 - Python 3.11](https://www.python.org/downloads/)
- [PIP](https://pip.pypa.io/en/stable/installation/)

## 设置您的虚拟环境

无论您使用哪种包管理工具，我们建议在虚拟环境中运行本页面上的命令。这样可以确保为Streamlit拉取的依赖不会影响您正在处理的其他Python项目。

以下是一些您可以用于环境管理的工具：

- [venv](https://docs.python.org/zh-cn/3/library/venv.html)
- [pipenv](https://pipenv-fork.readthedocs.io/en/latest/)
- [poetry](https://python-poetry.org/)
- [virtualenv](https://virtualenv.pypa.io/en/latest/)
- [conda](https://www.anaconda.com/distribution/)

## 在Windows上安装Streamlit

Streamlit在Windows上的官方支持的环境管理工具是[Anaconda Navigator](https://docs.anaconda.com/free/navigator/)。

### 安装Anaconda

如果您还没有安装Anaconda，请按照[Anaconda安装页面](https://docs.anaconda.com/free/anaconda/install/windows/)上提供的步骤进行安装。

### 使用Streamlit创建一个新的环境

接下来您需要设置环境。

1. 按照Anaconda提供的步骤，使用Anaconda Navigator [设置和管理您的环境](https://docs.anaconda.com/free/navigator/getting-started/#navigator-managing-environments)。

2. 在您的新环境旁边选择"▶"图标。然后选择"打开终端"：

   !["在Anaconda Navigator中打开终端"](https://i.stack.imgur.com/EiiFc.png)

3. 在出现的终端中，输入以下命令：

   ```bash
   pip install streamlit
   ```

4. 测试安装是否成功:

   ```bash
   streamlit hello
   ```

   Streamlit的Hello应用程序应该会在您的Web浏览器中的一个新标签页中显示！

### 使用您的新环境

1. 在Anaconda Navigator中，在您的环境中打开一个终端（参见上述第2步）。
2. 在出现的终端中，像往常一样使用Streamlit:

   ```bash
   streamlit run myfile.py
   ```

## 在macOS/Linux上安装Streamlit

Streamlit官方支持的macOS和Linux的包管理器和环境管理器分别是[pip](https://pypi.org/project/pip/)和[venv](https://docs.python.org/3/library/venv.html)。`venv`是[Python标准库](https://docs.python.org/3/library/index.html)的一部分，并且已经随Python安装包一起提供。请参阅下面的说明以了解如何安装和使用`pip`。

### 安装`pip`

安装`pip`。关于安装`pip`的更多细节，请参阅[pip的文档](https://pip.pypa.io/en/stable/installation/#supported-methods)。

在macOS上：

```bash
python -m ensurepip --upgrade
```

在Ubuntu上使用Python 3：

```bash
sudo apt-get install python3-pip
```

对于其他Linux发行版，请参阅[如何安装Python的PIP](https://www.makeuseof.com/tag/install-pip-for-python/)。

### 在macOS上安装Xcode命令行工具

在macOS上，您需要安装Xcode命令行工具。在安装过程中，它们需要编译一些Streamlit的Python依赖项。要安装Xcode命令行工具，请运行：

```bash
xcode-select --install
```

### 使用 Streamlit 创建新的环境

1. 导航到您的项目文件夹:

   ```bash
   cd myproject
   ```

2. 在该文件夹中创建一个新的虚拟环境并激活该环境:

   ```bash
   python -m venv .venv
   ```

   当您运行上述命令时，一个名为 `.venv` 的目录将出现在 `myproject/` 中。这个目录是您的虚拟环境及其依赖项的安装位置。

3. 在您的环境中安装 Streamlit:

   ```bash
   pip install streamlit
   ```

4. 测试安装是否成功：

   ```bash
   streamlit hello
   ```

   Streamlit的Hello应用程序应该在您的网页浏览器中的一个新标签页中显示！

   <Cloud src="https://doc-mpa-hello.streamlit.app/?embed=true" height="700" />

### 使用您的新环境

1. 每次想要使用新环境时，您首先需要进入您的项目文件夹（包含`.venv`目录）并运行：

   ```bash
   source .venv/bin/activate
   ```

2. 现在您可以像往常一样使用Python和Streamlit:

   ```bash
   streamlit run myfile.py
   ```

   要停止Streamlit服务器，请按下`ctrl-C`。

3. 当您完成使用此环境后，输入`deactivate`以返回到正常的shell。

现在您已经安装了Streamlit，请花几分钟阅读[主要概念](/library/get-started/main-concepts)以了解Streamlit的数据流模型。