---
title: 嵌入您的应用
slug: /streamlit-community-cloud/get-started/embed-your-app
---

# 嵌入您的应用

通过嵌入[Streamlit Community Cloud](https://streamlit.io/cloud)应用程序，可以在页面中直接集成交互式的、数据驱动的应用程序，从而丰富您的内容。无论您是撰写博客文章、技术文档，还是在Medium、Notion或StackOverflow等平台上共享资源，嵌入Streamlit应用程序都可以为您的内容增加动态组件。这样，您的受众可以与您的想法进行互动，而不仅仅是阅读或查看截图。

Streamlit社区云同时支持使用[iframe](#embedding-with-iframes)和[oEmbed](#embedding-with-oembed)方法来嵌入**公共**应用程序。这种灵活性使您能够在广泛的平台上共享您的应用程序，扩大应用程序的可见性和影响力。在本指南中，我们将介绍如何有效地使用这两种方法来与世界分享您的Streamlit应用程序。

## 使用iframe嵌入

Streamlit社区云支持使用子域名方案嵌入**公共**应用程序。要嵌入公共应用程序，请在`*streamlit.app`的URL末尾添加查询参数`/?embed=true`。

例如，假设您想要嵌入[30DaysOfStreamlit应用程序](https://30days.streamlit.app/)。您需要在iframe中包含的URL是：[https://30days.streamlit.app/?embed=true](https://30days.streamlit.app/?embed=true)。

```javascript
<iframe
  src="https://30days.streamlit.app/?embed=true"
  height="450"
  style="width:100%;border:none;"
></iframe>
```

<Cloud src="https://30days.streamlit.app/?embed=true" />

<Important>

不支持嵌入私有应用。

</Important>

除了通过iframe嵌入应用外，`?embed=true`查询参数还具有以下功能：

- 移除带有汉堡菜单的工具栏
- 移除应用程序顶部和底部的填充
- 移除页脚
- 移除应用程序顶部的彩色线条

对于对嵌入行为有更精细控制需求的情况，Streamlit允许您指定一个或多个`?embed_options`查询参数实例（例如，显示工具栏，以暗色主题打开应用程序等）。[单击此处查看完整的嵌入选项列表。](#embed-options)

## 使用oEmbed嵌入

新的oEmbed支持功能使嵌入体验更加简单。现在，您可以直接将Streamlit应用的URL放入Medium、Ghost或Notion页面（或任何支持oEmbed或[embed.ly](https://embed.ly/)的网站，包括700多个内容提供商），嵌入的应用程序将自动显示。这有助于Streamlit社区云应用程序与这些平台无缝集成，提高应用程序的可见性和可访问性。

### 示例

在创建Notion页面、Medium文章或Ghost博客的内容时，您只需粘贴应用程序的URL并按Enter键。应用程序将自动在您的内容中的该位置进行渲染。使用与iframe嵌入相同的URL，但不包含`?embed=true`查询参数。

```
https://30days.streamlit.app/
```

以下是[@chrieke](https://github.com/chrieke)的Prettymapp应用程序的示例，嵌入在Medium文章中：

<Image src="/images/streamlit-community-cloud/oembed.gif" alt="oEmbed example" clean />

<Tip>

确保托管嵌入的Streamlit应用程序的平台支持oEmbed或[embed.ly](https://embed.ly/)。

</Tip>

### oEmbed的关键网站

oEmbed应该可以在多个平台上开箱即用，包括但不限于以下平台：

- [Medium](https://medium.com/)
- [Notion](https://notion.so/)
- [Looker](https://www.looker.com/)
- [Tableau](https://www.tableau.com/)
- [Ghost](https://ghost.org/)
- [Discourse](https://www.discourse.org/)
- [StackOverflow](https://stackoverflow.com/)
- [W3](https://www.w3schools.com/)
- [Reddit](https://www.reddit.com/)

请查阅具体平台的文档以验证对oEmbed的支持情况。

### iframe与oEmbed的区别

这两种方法之间唯一值得注意的区别是，使用iframe可以通过在下一节中描述的各种`?embed_options`来自定义应用程序的嵌入行为（例如显示工具栏、以暗主题打开应用程序等）。

## 嵌入选项

在使用iframe嵌入时，Streamlit允许您通过指定一个或多个`?embed_options`查询参数来对嵌入行为进行细粒度控制。下面列出了对`?embed_options`支持的值：

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
   您也可以组合参数使用：

```javascript
/?embed=true&embed_options=show_toolbar&embed_options=show_padding&embed_options=show_footer&embed_options=show_colored_line&embed_options=disable_scrolling
```

`?embed`和`?embed_options`参数对于[`st.experimental_get_query_params`](/library/api-reference/utilities/st.experimental_get_query_params)和[`st.experimental_set_query_params`](/library/api-reference/utilities/st.experimental_set_query_params)是不可见的。前者忽略了嵌入式查询参数并不返回它们，而后者不允许设置嵌入式查询参数。
