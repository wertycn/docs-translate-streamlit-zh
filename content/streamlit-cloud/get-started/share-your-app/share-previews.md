---
slug: /streamlit-community-cloud/get-started/share-your-app/share-previews
title: Share previews
---

# 分享预览

在社交媒体网站上分享链接时，会生成一个带有标题、预览图片和描述的卡片。这个功能被称为"分享预览"。同样地，当您在社交媒体上分享一个公开的Streamlit应用程序的链接时，也会生成一个分享预览。以下是一个在Twitter上发布的公开Streamlit应用程序的分享预览示例：

<div style={{ marginLeft: '3em' }}>
    <Flex>
    <Image caption="共享预览公共Streamlit应用程序" src="/images/streamlit-community-cloud/share-preview-twitter-annotated.png" />
    </Flex>
</div>

<Note>

共享预览仅针对在Community Cloud上部署的公共应用程序生成。

</Note>

## 标题

标题是在分享预览顶部显示的文本。当您访问应用程序时，该文本也会显示在浏览器标签中。您应该将标题设置为对您的应用程序受众有意义且能描述应用程序功能的内容。最佳实践是将标题保持简洁，最好不超过60个字符。

设置分享预览标题有两种方式：

1. 在 [`st.set_page_config()`](/library/api-reference/utilities/st.set_page_config) 中设置 `page_title` 参数为您想要的标题。例如：

   ```python
   import streamlit as st

   st.set_page_config(page_title="我的应用")

   # ... 应用的其余部分
   ```

2. 如果您没有设置`page_title`参数，分享预览的标题将会是您的应用程序的 GitHub 仓库名称。例如，如果您在`st.set_page_config()`中没有设置`page_title`参数，那么在 GitHub 上托管的应用程序（例如：https://github.com/jrieke/traingenerator）的分享预览标题将会是"traingenerator"。

## 描述

描述是在分享预览中标题下方显示的文本。描述应该总结应用程序的功能，并且最好不超过100个字符。

Streamlit从应用程序的GitHub存储库中的README中提取描述。如果没有README，描述将默认为：

_这个应用程序是用Streamlit构建的！查看它并访问https://streamlit.io以获取更多令人惊叹的社区应用程序。🎈_

<div style={{ marginLeft: '6em' }}>
    <Flex>
    <div>
    <Flex>
        <Image caption="当描述缺失时的默认分享预览" src="/images/streamlit-community-cloud/share-preview-private-app.png" />
    </Flex>
</div>

<!-- ![当描述缺失时的默认分享预览](/images/streamlit-community-cloud/share-preview-private-app.png) -->

如果您希望分享预览看起来很棒，并且希望用户分享您的应用程序并点击您的链接，您应该在应用程序的GitHub存储库的README中编写一个好的描述。

## 预览图片

Community Cloud每天对您的应用程序进行一次截图，并将其用作预览图片，与标题和描述不同，标题和描述是直接从您的应用程序代码或GitHub存储库中提取的。此截图可能需要最多24小时才能更新。

## 将您的应用程序从公开切换到私有

如果您最初将应用程序设置为公开，并且后来决定将其设置为私有，我们将停止为该应用程序生成共享预览。但是，共享预览可能需要最多24小时才能停止显示。