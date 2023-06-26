---
slug: /knowledge-base/using-streamlit/how-do-i-run-my-streamlit-script
title: How do I run my Streamlit script?
---

# 如何运行我的Streamlit脚本？

使用Streamlit非常简单。首先，您需要在一个普通的Python脚本中添加一些Streamlit命令，然后运行它。根据您的使用情况，以下列出了一些运行脚本的方式。

## 使用streamlit run

在创建好脚本后，比如`your_script.py`，最简单的运行方式是使用`streamlit run`命令：

```bash
streamlit run your_script.py
```

当您按照上面所示的方式运行脚本时，一个本地的Streamlit服务器将会启动，并且您的应用程序将在默认的Web浏览器中的一个新标签页中打开。

### 向脚本传递参数

当传递一些自定义参数给您的脚本时，它们必须在两个短划线之后传递。否则这些参数会被解释为Streamlit自身的参数。

```bash
streamlit run your_script.py [-- script args]
```

### 通过URL传递给streamlit run

您还可以将URL传递给`streamlit run`！当您的脚本托管在远程位置时（例如GitHub Gist），这非常有用。例如：

```bash
streamlit run https://raw.githubusercontent.com/streamlit/demo-uber-nyc-pickups/master/streamlit_app.py
```

## 将 Streamlit 作为 Python 模块运行

另一种运行 Streamlit 的方式是将其作为 Python 模块运行。当配置像 PyCharm 这样的 IDE 与 Streamlit 一起使用时，这种方式非常有用：

```bash
# Running
python -m streamlit run your_script.py
```

```bash
# is equivalent to:
streamlit run your_script.py
```