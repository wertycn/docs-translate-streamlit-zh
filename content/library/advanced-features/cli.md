---
title: 命令行选项
slug: /library/advanced-features/cli
---

# 命令行界面

安装Streamlit时，还会安装一个命令行（CLI）工具。该工具的目的是运行Streamlit应用程序，更改Streamlit配置选项，并帮助您诊断和修复问题。

要查看所有支持的命令，请执行以下操作：

```bash
streamlit --help
```

### 运行Streamlit应用程序

```bash
streamlit run your_script.py [-- script args]
```

运行您的应用程序。您可以随时使用**Ctrl+c**停止服务器。

<注意>

当传递一些自定义参数给您的脚本时，**它们必须在两个破折号之后传递**。否则，这些参数会被解释为Streamlit本身的参数。

</注意>

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

在 web 浏览器中打开 Streamlit 文档（即此网站）。

### 清除缓存

```bash
streamlit cache clear
```

清除磁盘上的持久化文件，如果存在的话，这些文件是存储在[Streamlit缓存](/library/api-reference/performance)中的。

### 查看所有配置选项

如[配置](/library/advanced-features/configuration)中所述，Streamlit有几个配置选项。要查看所有选项，包括它们的当前值，只需输入：

```bash
streamlit config show
```
