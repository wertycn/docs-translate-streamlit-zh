---
slug: /library/advanced-features/cli
title: Command-line options
---

# 命令行界面

安装Streamlit时，会同时安装一个命令行工具（CLI）。该工具的目的是运行Streamlit应用程序，更改Streamlit配置选项，并帮助您诊断和修复问题。

要查看所有支持的命令：

```bash
streamlit --help
```

### 运行Streamlit应用程序

```bash
streamlit run your_script.py [-- script args]
```

运行您的应用程序。您可以随时使用**Ctrl+c**停止服务器。

<Note>

当传递一些自定义参数给您的脚本时，**它们必须在两个破折号后面传递**，否则这些参数会被解释为Streamlit本身的参数。

</Note>

要查看Streamlit的'Hello, World!'示例应用程序，请运行`streamlit hello`。

### 查看Streamlit版本

要查看安装的Streamlit版本，只需键入：

```bash
streamlit version
```

### 查看文档

```bash
streamlit docs
```

在Web浏览器中打开Streamlit文档（即本网站）。

### 清除缓存

```bash
streamlit cache clear
```

清除存储在磁盘上的[Streamlit缓存](/library/api-reference/performance)中的文件，如果有的话。

### 查看所有配置选项

如[配置](/library/advanced-features/configuration)中所述，Streamlit有几个配置选项。要查看所有配置选项，包括它们的当前值，只需输入：

```bash
streamlit config show
```