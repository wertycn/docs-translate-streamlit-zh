---
slug: /knowledge-base/deploy/invoking-python-subprocess-deployed-streamlit-app
title: Invoking a Python subprocess in a deployed Streamlit app
---

# 在部署的Streamlit应用中调用Python子进程

## 问题

假设您想在部署的Streamlit应用程序`streamlit_app.py`中调用子进程来运行Python脚本`script.py`。例如，使用命令行界面运行机器学习库[Ludwig](https://ludwig-ai.github.io/ludwig-docs/)，或者您想从Python中运行bash脚本或类似类型的进程。

您已经尝试了以下操作，但在`script.py`中遇到了依赖问题，尽管您已经在要求文件中指定了Python依赖项:

```python
# streamlit_app.py
import streamlit as st
import subprocess

subprocess.run(["python", "script.py"])
```

## 解决方案

当您运行上述代码块时，您将得到系统路径上的Python版本，而不一定是运行Streamlit代码所在的虚拟环境中安装的Python可执行文件。

解决方案是使用[`sys.executable`](https://docs.python.org/3/library/sys.html#sys.executable)直接检测Python可执行文件：

```python
# streamlit_app.py
import streamlit as st
import subprocess
import sys

subprocess.run([f"{sys.executable}", "script.py"])
```

这确保了`script.py`在与您的Streamlit代码相同的Python可执行文件下运行，其中安装了您的[Python依赖项](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)。

### 相关链接

- https://stackoverflow.com/questions/69947867/run-portion-of-python-code-in-parallel-from-a-streamlit-app/69948545#69948545
- https://discuss.streamlit.io/t/modulenotfounderror-no-module-named-cv2-streamlit/18319/3?u=snehankekre
- [https://docs.python.org/3/library/sys.html#sys.executable](https://docs.python.org/3/library/sys.html#sys.executable)