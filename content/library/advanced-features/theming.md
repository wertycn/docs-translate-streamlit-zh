---
slug: /library/advanced-features/theming
title: Theming
---

# 主题

在本指南中，我们提供了一些示例，展示了各种主题配置选项对Streamlit页面元素的影响。关于Streamlit主题的更高级概述，请参阅[主要概念文档](/library/get-started/main-concepts#themes)中的主题部分。

Streamlit主题使用常规配置选项进行定义：可以通过在启动应用程序时使用`streamlit run`命令行标志设置主题，也可以通过
在`.streamlit/config.toml`文件的`[theme]`部分定义它。有关设置配置选项的更多信息，请参阅[Streamlit配置文档](/library/advanced-features/configuration#set-configuration-options)。

以下配置选项显示了在`.streamlit/config.toml`文件的`[theme]`部分中重新创建的默认Streamlit Light主题。

```toml
[theme]
primaryColor="#F63366"
backgroundColor="#FFFFFF"
secondaryBackgroundColor="#F0F2F6"
textColor="#262730"
font="sans serif"
```

让我们逐个介绍这些选项，并提供截图来演示它们在Streamlit应用程序中的影响部分。

## primaryColor

`primaryColor`定义了在Streamlit应用程序中最常用的强调颜色。一些使用`primaryColor`的Streamlit小部件的示例包括`st.checkbox`，`st.slider`和`st.text_input`（当聚焦时）。

![主要颜色](/images/theme_config_options/primaryColor.png)

<Tip>

任何CSS颜色都可以作为primaryColor和其他颜色选项的值使用。这意味着主题颜色可以使用十六进制或浏览器支持的颜色名称（如“green”，“yellow”和“chartreuse”）来指定。甚至可以使用RGB和HSL格式来定义它们！

## backgroundColor

定义应用程序主内容区域的背景颜色。

## secondaryBackgroundColor

在需要第二个背景颜色的地方使用此颜色以获得更多的...
对比度。最显著的是侧边栏的背景颜色。它还用作大多数交互式小部件的背景颜色。

![次要背景颜色](/images/theme_config_options/secondaryBackgroundColor.png)

## 文本颜色

此选项控制大多数Streamlit应用程序的文本颜色。

## 字体

选择在Streamlit应用程序中使用的字体。有效的值为`"sans serif"`、`"serif"`和`"monospace"`。如果未设置或无效，此选项默认为`"sans serif"`。

注意，代码块始终使用等宽字体进行渲染，而不受此处选择的字体的影响。

## base

定义自定义主题并对预设的Streamlit主题进行小的更改的简单方法是使用`base`选项。通过以下方式编写代码，可以将Streamlit Light主题重新创建为自定义主题：

```toml
[theme]
base="light"
```

`base`选项允许您指定一个预设的Streamlit主题，您的自定义主题会继承该主题。在您的主题设置中未定义的任何主题配置选项都将使用基础主题的值。`base`的有效值为`"light"`和`"dark"`。

例如，以下主题配置定义了一个几乎与Streamlit Dark主题相同的自定义主题，但具有新的`primaryColor`。

```toml
[theme]
base="dark"
primaryColor="purple"
```

如果`base`本身被省略，则默认为`"light"`，因此您可以定义一个自定义主题，将Streamlit Light主题的字体更改为衬线字体，配置如下：

```toml
[theme]
font="serif"
```