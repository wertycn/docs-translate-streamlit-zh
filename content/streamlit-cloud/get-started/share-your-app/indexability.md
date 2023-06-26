---
slug: /streamlit-community-cloud/get-started/share-your-app/indexability
title: App indexability
---

# 应用程序索引性

当您将一个公共应用程序部署到Community Cloud时，它会每周自动被搜索引擎（如Google和Bing）索引。🎈 这意味着任何人都可以通过搜索其自定义子域名（例如<https://traingenerator.streamlit.app>）或搜索应用程序的标题来找到您的应用程序。

## 充分利用应用程序索引性

以下是一些帮助您充分利用应用程序索引性的提示：

1. [确保您的应用程序是公开的](#确保您的应用程序是公开的)
2. [尽早选择自定义子域名](#选择自定义子域名-early)
3. [选择一个描述性的应用标题](#选择一个描述性的应用标题)
4. [自定义应用的元描述](#自定义应用的元描述)

### 确保您的应用程序是公开的

所有托管在Community Cloud上的公共应用程序都会被搜索引擎索引。如果您的应用程序是私有的，则不会被搜索引擎索引。要使您的私有应用程序变为公开，请阅读[分享您的应用程序](/streamlit-community-cloud/get-started/share-your-app)。

### 提早选择自定义子域名

Community Cloud会根据应用程序的GitHub仓库结构自动生成一个随机子域名。要了解有关应用程序URL的更多信息，请参阅[Your app URL](/streamlit-community-cloud/get-started/deploy-an-app#your-app-url)。但是，子域名是可以自定义的！定制子域名可以修改您的应用程序URL以反映您的应用程序内容、个人品牌或任何您想要的内容。在[Custom subdomains](streamlit-cloud/get-started/deploy-an-app#custom-subdomains)中了解更多关于自定义子域名的信息。

通过选择一个自定义子域名，您可以用它来帮助人们找到您的应用程序。例如，如果您正在部署一个生成训练数据的应用程序，您可以选择一个类似于 `traingenerator.streamlit.app` 的子域名。这样，人们可以通过搜索“training generator”或“train generator streamlit app”来轻松找到您的应用程序。

我们建议您在部署应用程序后尽早选择自定义子域名。这样可以确保搜索引擎使用您的自定义子域名进行索引，而不是使用自动生成的子域名。如果您稍后选择自定义子域名，搜索引擎可能首先使用默认子域名对您的应用程序进行索引。这意味着您的应用程序将被多次索引，一次使用默认子域名（导致404错误），一次使用您的自定义子域名。这可能会让正在搜索您的应用程序的用户感到困惑。

### 选择一个描述性的应用标题

您的应用的元标题是在搜索引擎结果中显示的文本。它也是在您的应用打开时浏览器选项卡中显示的文本。默认情况下，您的应用的元标题与应用的标题相同。但是，您可以通过将[`st.set_page_config`](/library/api-reference/utilities/st.set_page_config)参数`page_title`设置为自定义字符串来自定义您的应用的元标题。例如：

```python
st.set_page_config(page_title="Traingenerator")
```

这将把您的应用的元标题更改为"Traingenerator"。这样可以方便人们通过搜索"Traingenerator"或"train generator streamlit app"来找到您的应用程序:

<Image src="/images/streamlit-community-cloud/indexability-app-title.png" caption='Google搜索结果中的"train generator streamlit app"' />

### 自定义应用的元描述

元描述是出现在搜索引擎结果中的简短描述。搜索引擎使用元描述来帮助用户了解您的应用程序的内容。

根据我们的观察，搜索引擎似乎更青睐`st.header`和`st.text`中的内容，而不是`st.title`。如果您在应用程序顶部的`st.header`或`st.text`下方放置了描述，搜索引擎很可能会将其用作元描述。

## 我的索引应用程序是什么样子的？

如果您对您的应用在搜索引擎结果中的显示效果感兴趣，您可以在Google搜索中输入以下内容:

```
site:<your-app-url>
```

示例：`site:traingenerator.streamlit.app`

<Image src="/images/streamlit-community-cloud/indexability-search-result.png" caption='对于"site:traingenerator.streamlit.app"的Google搜索结果' />

## 如果我不想让我的应用被索引怎么办？

如果您不希望您的应用程序被搜索引擎索引，您可以将其设置为私有。阅读[分享您的应用程序](/streamlit-community-cloud/get-started/share-your-app)以了解更多关于如何将应用程序设置为私有的信息。注意：每个工作区只能拥有一个私有应用程序。如果您想将应用程序设置为私有，请先删除工作区中的其他私有应用程序。

话虽如此，Community Cloud 是一个开放和免费的平台，供社区成员部署、发现和共享 Streamlit 应用和代码。因此，我们鼓励您将您的应用程序设为公开，以便被搜索引擎索引并被其他 Streamlit 用户和社区成员发现。

## 有问题或反馈意见吗？

如果您在应用索引性能方面遇到问题，或者对此有任何问题或反馈，我们非常乐意听到您的意见！请在我们的[社区论坛](https://discuss.streamlit.io)上发帖，以便我们的团队和社区成员能够帮助您。🤗