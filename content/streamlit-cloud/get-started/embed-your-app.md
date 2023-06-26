---
slug: /streamlit-community-cloud/get-started/embed-your-app
title: Embed your app
---

# 嵌入您的应用程序

嵌入[Streamlit Community Cloud](https://streamlit.io/cloud)应用程序可以通过将交互式的、数据驱动的应用程序直接集成到您的页面中，丰富您的内容。无论您是在撰写博客文章、技术文档，还是在像Medium、Notion甚至StackOverflow这样的平台上共享资源，嵌入Streamlit应用程序都可以为您的内容增加动态组件。这使得您的受众可以与您的想法进行交互，而不仅仅是阅读或查看截屏。

Streamlit社区云支持使用[iframe](#使用iframe嵌入)和[oEmbed](#使用oEmbed嵌入)方法来嵌入**公开**应用程序。这种灵活性使您能够在广泛的平台上共享应用程序，扩大应用程序的可见性和影响力。在本指南中，我们将介绍如何有效地使用这两种方法来与世界分享您的Streamlit应用程序。

## 使用iframe嵌入

Streamlit社区云支持使用子域名方案嵌入**公开**应用程序。要嵌入公开的应用程序，请在`*streamlit.app` URL的末尾添加查询参数`/?embed=true`。

例如，假设您想要嵌入[30DaysOfStreamlit应用程序](https://30days.streamlit.app/)。您应该在iframe中包含以下URL：[https://30days.streamlit.app/?embed=true](https://30days.streamlit.app/?embed=true)。

```javascript
<iframe
  src="https://30days.streamlit.app/?embed=true"
  height="450"
  style="width:100%;border:none;"
></iframe>
```

<Cloud src="https://30days.streamlit.app/?embed=true" />

<Important>

不支持嵌入私有应用程序。

</Important>

除了通过iframe嵌入应用程序外，`?embed=true`查询参数还具有以下功能：

- 移除带有汉堡菜单的工具栏
- 移除应用程序顶部和底部的填充
- 移除页脚
- 移除应用程序顶部的彩色线条

要对嵌入行为进行更精细的控制，Streamlit允许您指定一个或多个`?embed_options`查询参数的实例（例如显示工具栏、在深色主题中打开应用程序等）。[点击此处查看完整的嵌入选项列表。](#embed-options)

## 使用oEmbed进行嵌入

新的oEmbed支持可以实现更简单的嵌入体验。现在，您可以直接将Streamlit应用程序的URL放入Medium、Ghost或Notion页面（或任何支持oEmbed或[embed.ly](https://embed.ly/)的网站，包括700多家内容提供商），嵌入的应用程序将自动显示。这有助于Streamlit社区云应用程序与这些平台无缝集成，提高应用程序的可见性和可访问性。

### 示例

在Notion页面、Medium文章或Ghost博客中创建内容时，您只需要粘贴应用程序的URL并按Enter键。然后，应用程序将自动在您的内容中的该位置进行渲染。使用与iframe嵌入相同的URL，但不包含`?embed=true`查询参数。

```
https://30days.streamlit.app/
```

这是一个[@chrieke](https://github.com/chrieke)的Prettymapp应用程序的示例，嵌入在Medium文章中：

<Image src="/images/streamlit-community-cloud/oembed.gif" alt="oEmbed示例" clean />

<Tip>

确保托管嵌入的Streamlit应用程序的平台支持oEmbed或[embed.ly](https://embed.ly/)。

</Tip>

### oEmbed的关键网站

oEmbed应该在多个平台上可以直接使用，包括但不限于：

- [Medium](https://medium.com/)
- [Notion](https://notion.so/)
- [Looker](https://www.looker.com/)
- [Tableau](https://www.tableau.com/)
- [Ghost](https://ghost.org/)
- [Discourse](https://www.discourse.org/)
- [StackOverflow](https://stackoverflow.com/)
- [W3](https://www.w3schools.com/)
- [Reddit](https://www.reddit.com/)

请查看具体平台的文档以验证对oEmbed的支持。

### iframe与oEmbed

这两种方法之间唯一值得注意的区别是，使用iframe可以通过使用下一节中描述的各种`?embed_options`来自定义应用程序的嵌入行为（例如显示工具栏、在深色主题中打开应用程序等）。

## 嵌入选项

在使用iframe嵌入时，Streamlit允许您通过`?embed_options`查询参数来指定一个或多个实例，以对嵌入行为进行细粒度控制。支持的`?embed_options`值如下所示：

1. 显示工具栏

   ```javascript
   /?embed=true&embed_options=show_toolbar
   ```

2. 显示填充

   ```javascript
   /?embed=true&embed_options=show_padding
   ```

3. 显示页脚

   ```javascript
   您还可以组合参数使用：

```javascript
/?embed=true&embed_options=show_toolbar&embed_options=show_padding&embed_options=show_footer&embed_options=show_colored_line&embed_options=disable_scrolling
```

[`st.experimental_get_query_params`](/library/api-reference/utilities/st.experimental_get_query_params)和[`st.experimental_set_query_params`](/library/api-reference/utilities/st.experimental_set_query_params)都对`?embed`和`?embed_options`是不可见的。前者忽略嵌入查询参数并不返回它们，而后者不允许设置嵌入查询参数。