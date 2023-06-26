---
标题: 主题
链接: /library/advanced-features/theming
---

# 主题

在本指南中，我们提供了示例，展示了Streamlit页面元素如何受到各种主题配置选项的影响。如果您想了解更高层次的Streamlit主题概述，请参阅
[主要概念文档](/library/get-started/main-concepts#themes)中的主题部分。

Streamlit主题是使用常规配置选项进行定义的：您可以通过命令行标志在启动应用程序时使用`streamlit run`设置主题，也可以通过其他方式进行设置。
在`.streamlit/config.toml`文件的`[theme]`部分定义它。有关设置配置选项的更多信息，请参阅[Streamlit配置文档](/library/advanced-features/configuration#set-configuration-options)。

以下配置选项展示了在`.streamlit/config.toml`文件的`[theme]`部分中重新创建的默认Streamlit Light主题。

```toml
[theme]
primaryColor="#F63366"
backgroundColor="#FFFFFF"
secondaryBackgroundColor="#F0F2F6"
textColor="#262730"
font="sans serif"
```

让我们逐个介绍这些选项，并在需要时提供截图来演示它们在Streamlit应用程序中的作用。

## primaryColor

`primaryColor` 定义了在Streamlit应用程序中经常使用的强调颜色。一些使用 `primaryColor` 的 Streamlit 小部件的例子包括 `st.checkbox`、`st.slider` 和 `st.text_input`（在焦点状态下）。

![主颜色](/images/theme_config_options/primaryColor.png)

<Tip>

任何CSS颜色都可以用作primaryColor和下面的其他颜色选项的值。这意味着可以使用十六进制或浏览器支持的颜色名称（如“green”，“yellow”和“chartreuse”）来指定主题颜色。它们甚至可以用RGB和HSL格式来定义！

## backgroundColor

定义应用程序主内容区域的背景颜色。

## secondaryBackgroundColor

在需要第二个背景颜色的地方使用此颜色。
对比。最明显的是侧边栏的背景颜色。它也被用作大多数交互式小部件的背景颜色。

![次要背景颜色](/images/theme_config_options/secondaryBackgroundColor.png)

## 文本颜色

此选项控制您Streamlit应用程序中大多数文本的颜色。

## 字体

选择在您的Streamlit应用程序中使用的字体。有效值为`"sans serif"`、`"serif"`和`"monospace"`。如果未设置或无效，此选项默认为`"sans serif"`。

请注意，无论在这里选择的字体如何，代码块始终使用等宽字体进行渲染。

## base

定义自定义主题并对预设的Streamlit主题进行小的更改的简单方法是使用`base`选项。通过以下代码，可以将Streamlit Light主题重新创建为自定义主题：

```toml
[theme]
base="light"
```

`base`选项允许您指定一个预设的Streamlit主题，您的自定义主题将继承自该主题。在您的主题设置中未定义的任何主题配置选项都将使用基础主题的值。`base`的有效值为`"light"`和`"dark"`。

例如，以下主题配置定义了一个几乎与Streamlit Dark主题相同的自定义主题，但具有新的`primaryColor`。

```toml
[theme]
base="dark"
primaryColor="purple"
```

如果省略`base`，它默认为`"light"`，因此您可以定义一个自定义主题，将Streamlit Light主题的字体更改为serif，使用以下配置：

```toml
[theme]
font="serif"
```
