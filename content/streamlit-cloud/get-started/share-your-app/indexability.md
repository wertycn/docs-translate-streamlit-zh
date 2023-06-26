---
title: 应用索引性
slug: /streamlit-community-cloud/get-started/share-your-app/indexability
---

# 应用索引性

当您将一个公共应用程序部署到Community Cloud时，它会每周自动被搜索引擎（如Google和Bing）索引。🎈 这意味着任何人都可以通过搜索其自定义子域名（例如 <https://traingenerator.streamlit.app>）或搜索应用程序的标题来找到您的应用。

## 充分利用应用索引性

以下是一些帮助您充分利用应用索引性的提示：

1. [确保您的应用程序是公开的](#确保您的应用程序是公开的)
2. [尽早选择自定义子域名](#尽早选择自定义子域名)
3. [选择一个描述性的应用程序标题](#选择一个描述性的应用程序标题)
4. [自定义您的应用程序的元描述](#自定义您的应用程序的元描述)

### 确保您的应用程序是公开的

所有托管在Community Cloud上的公共应用程序都会被搜索引擎索引。如果您的应用程序是私有的，它将不会被搜索引擎索引。要使您的私有应用程序公开，阅读[分享您的应用程序](/streamlit-community-cloud/get-started/share-your-app)。

### 尽早选择自定义子域名

Community Cloud会根据应用的GitHub存储库的结构自动生成一个随机子域名。要了解有关应用程序URL的更多信息，请参阅[您的应用程序URL](/streamlit-community-cloud/get-started/deploy-an-app#your-app-url)。但是，子域名是可自定义的！自定义子域名可修改您的应用程序URL以反映您的应用程序内容、个人品牌或任何您喜欢的内容。请在[自定义子域名](streamlit-cloud/get-started/deploy-an-app#custom-subdomains)中了解更多关于自定义子域名的信息。

通过选择一个自定义子域名，您可以用它来帮助人们找到您的应用程序。例如，如果您正在部署一个生成训练数据的应用程序，您可以选择一个类似于 `traingenerator.streamlit.app` 的子域名。这样，人们可以通过搜索 "training generator" 或 "train generator streamlit app" 来轻松找到您的应用程序。

我们建议在部署应用程序后尽早选择自定义子域名。这样可以确保搜索引擎使用您的自定义子域名对应用程序进行索引，而不是使用自动生成的默认子域名。如果您稍后选择自定义子域名，您的应用程序可能会首先被搜索引擎使用默认子域名进行索引。这意味着您的应用程序将被索引多次，一次使用默认子域名（这将导致404错误），一次使用您的自定义子域名。这可能会让正在搜索您的应用程序的用户感到困惑。

### 选择一个描述性的应用标题

应用的元标题是在搜索引擎结果中显示的文本。当您的应用程序打开时，它还是在浏览器标签中显示的文本。默认情况下，应用的元标题与应用的标题相同。但是，您可以通过将[`st.set_page_config`](/library/api-reference/utilities/st.set_page_config)参数`page_title`设置为自定义字符串来自定义应用的元标题。例如：

```python
st.set_page_config(page_title="Traingenerator")
```

这将把您的应用的元标题更改为“Traingenerator”。这样，人们可以通过搜索“Traingenerator”或“train generator streamlit app”更容易地找到您的应用程序：

![Google search results for "train generator streamlit app"](https://example.com/images/streamlit-community-cloud/indexability-app-title.png)

### 自定义应用程序的元描述

元描述是出现在搜索引擎结果中的简短描述。搜索引擎使用元描述来帮助用户了解您的应用程序的内容。

根据我们的观察，搜索引擎似乎更偏爱`st.header`和`st.text`中的内容，而不是`st.title`。如果您在应用程序的顶部在`st.header`或`st.text`下方放置一个描述，搜索引擎很可能会将其用作元描述。

## 我的索引应用程序是什么样的？

如果您想知道您的应用在搜索引擎结果中的显示效果，可以在Google搜索中输入以下内容：

```
site:<your-app-url>
```

示例: `site:traingenerator.streamlit.app`

<Image src="/images/streamlit-community-cloud/indexability-search-result.png" caption='对于"site:traingenerator.streamlit.app"的Google搜索结果' />

## 如果我不想让我的应用被索引怎么办？

如果您不希望您的应用被搜索引擎索引，您可以将其设为私有。阅读[分享您的应用](/streamlit-community-cloud/get-started/share-your-app)以了解更多关于如何将您的应用设为私有的信息。注意：每个工作区只能有一个私有应用。如果您想将您的应用设为私有，您必须先删除工作区中的任何其他私有应用。

说了这么多，社区云是一个开放和免费的平台，供社区中的人部署、发现和共享Streamlit应用程序和代码。因此，我们鼓励您将您的应用程序公开，以便它可以被搜索引擎索引，并被其他Streamlit用户和社区成员发现。

## 有问题或反馈吗？

如果您在应用索引性方面遇到问题，或者有任何疑问或反馈意见，我们非常愿意听到您的声音！请在我们的[社区论坛](https://discuss.streamlit.io)上发布，这样我们的团队和社区成员可以为您提供帮助。🤗
