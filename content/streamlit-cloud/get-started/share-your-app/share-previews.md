---
title: 分享预览
slug: /streamlit-community-cloud/get-started/share-your-app/share-previews
---

# 分享预览

当您分享链接时，社交媒体网站会生成一个带有标题、预览图片和描述的卡片。这个功能称为"分享预览"。同样地，当您在社交媒体上分享一个公开的Streamlit应用程序的链接时，也会生成一个分享预览。以下是一个在Twitter上发布的公开Streamlit应用的分享预览示例：

<div style={{ marginLeft: '3em' }}>
    <Flex>
    <Image caption="公共Streamlit应用的分享预览" src="/images/streamlit-community-cloud/share-preview-twitter-annotated.png" />
    </Flex>
</div>

<Note>

分享预览仅适用于部署在Community Cloud上的公共应用。

</Note>

## 标题

标题是在共享预览顶部显示的文本。当您访问应用程序时，该文本也会显示在浏览器选项卡中。您应该将标题设置为对您的应用程序的受众有意义并描述应用程序的内容的内容。最佳实践是将标题保持简洁，理想情况下不超过60个字符。

设置共享预览标题有两种方法：

1. 在[`st.set_page_config()`](/library/api-reference/utilities/st.set_page_config)中设置`page_title`参数为您所需的标题。例如:

   ```python
   import streamlit as st

   st.set_page_config(page_title="我的应用")

   # ... 其余部分的应用程序
   ```

2. 如果您不设置`page_title`参数，分享预览的标题将会是您应用程序的GitHub仓库的名称。例如，如果您在`st.set_page_config()`中不设置`page_title`参数，那么在https://github.com/jrieke/traingenerator 上托管的应用程序的分享预览标题将会是"traingenerator"。

## 描述信息

描述是在分享预览中标题下方显示的文本。描述应该总结应用程序的功能，并且最好不超过100个字符。

Streamlit从应用程序的GitHub存储库中的README中获取描述。如果没有README，则描述将默认为：

_该应用程序是使用Streamlit构建的！快来看看，并访问https://streamlit.io获取更多精彩的社区应用程序。🎈_

<div style={{ marginLeft: '6em' }}>
    <Flex>
    <Image caption="当缺少描述时的默认分享预览" src="/images/streamlit-community-cloud/share-preview-private-app.png" />
    </Flex>
</div>

<!-- ![当缺少描述时的默认分享预览](/images/streamlit-community-cloud/share-preview-private-app.png) -->

如果您希望分享预览看起来很好，并且希望用户分享您的应用并点击您的链接，您应该在应用程序的GitHub存储库的README中编写一个好的描述。

## 预览图片

Community Cloud每天截取一次您的应用程序的屏幕截图，并将其用作预览图片，与标题和描述不同，标题和描述是直接从您的应用程序代码或GitHub存储库中提取的。此屏幕截图可能需要最多24小时才能更新。

## 将您的应用程序从公开切换到私有

如果您最初将应用程序设为公开，而后决定将其改为私有，我们将停止为该应用程序生成共享预览。但是，共享预览停止出现可能需要最多24小时的时间。
