---
title: 如何运行我的Streamlit脚本？
slug: /knowledge-base/using-streamlit/how-do-i-run-my-streamlit-script
---

# 如何运行我的Streamlit脚本？

使用Streamlit非常简单。首先在一个普通的Python脚本中添加一些Streamlit命令，然后运行它。根据您的使用情况，我们列出了几种运行脚本的方法。

## 使用streamlit run

一旦您创建了您的脚本，比如`your_script.py`，最简单的运行方式是使用`streamlit run`命令：

```bash
streamlit run your_script.py
```

当您按照上面所示运行脚本时，将启动一个本地的Streamlit服务器，并在默认的Web浏览器中打开您的应用程序的新标签页。

### 向您的脚本传递参数

当向您的脚本传递一些自定义参数时，它们必须在两个破折号之后传递。否则，这些参数会被解释为Streamlit本身的参数。

```bash
streamlit run your_script.py [-- script args]
```

### 传递URL给 streamlit run

您还可以将URL传递给 `streamlit run` 命令！当您的脚本托管在远程服务器上时，比如 GitHub Gist，这非常方便。例如：

```bash
streamlit run https://raw.githubusercontent.com/streamlit/demo-uber-nyc-pickups/master/streamlit_app.py
```

## 将Streamlit作为Python模块运行

另一种运行Streamlit的方式是将其作为Python模块运行。这在配置像PyCharm这样的IDE与Streamlit一起使用时非常有用。

```bash
# Running
python -m streamlit run your_script.py
```

```bash
# is equivalent to:
streamlit run your_script.py
```
