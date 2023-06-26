---
title: 部署应用程序
slug: /streamlit-community-cloud/get-started/deploy-an-app
---

# 部署应用程序

Streamlit社区云让您只需点击一次即可部署您的应用程序，大多数应用程序只需几分钟即可部署完成。如果您没有准备好部署的应用程序，您可以[分叉或克隆我们的示例应用程序之一](https://streamlit-cloud-example-apps-streamlit-app-sw3u0r.streamlit.app/?hsCtaTracking=28f10086-a3a5-4ea8-9403-f3d52bf26184|22470002-acb1-4d93-8286-00ee4f8a46fb) — 您可以找到用于机器学习、数据可视化、数据探索、A/B测试等的应用程序。

<Note>

如果您想在不同的云服务上部署您的应用程序，请查看我们知识库中的[部署Streamlit应用](/knowledge-base/tutorials/deploy)文章。

</Note>

## 将您的应用程序添加到GitHub

Streamlit社区云从您的GitHub存储库直接启动应用程序，因此在尝试部署应用程序之前，您的应用程序代码和依赖项需要在GitHub上。有关更多信息，请参阅[应用程序依赖关系](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies)。

### 可选：添加配置文件

Streamlit允许您通过四种不同的方法可选地设置配置选项。除其他事项外，您可以使用自定义配置来自定义应用程序的主题、启用日志记录或设置应用程序运行的端口。有关更多信息，请参阅[配置](/library/advanced-features/configuration)和[主题](/library/advanced-features/theming)。但是，在Streamlit Community Cloud上，您只能通过GitHub存储库中的配置文件来设置配置选项。

具体来说，您可以在存储库的根目录（顶级目录）中添加一个配置文件：创建一个名为`.streamlit`的文件夹，然后在该文件夹中添加一个名为`config.toml`的文件。例如，如果您的应用程序在名为`my-app`的存储库中，您将添加一个名为`my-app/.streamlit/config.toml`的文件。假设您想将应用程序的主题设置为“dark”模式。您可以将以下内容添加到您的`.streamlit/config.toml`文件中：

```toml
[theme]
base="dark"
```

<重要>

不论应用程序数量如何，只能有一个配置文件。

</重要>

## 部署您的应用程序

要部署一个应用程序，请从工作区的右上角点击“**新建应用程序**”。

![新建应用程序](/images/streamlit-community-cloud/deploy-empty-new-app.png)

填写您的仓库、分支和文件路径。作为快捷方式，您还可以点击“**粘贴GitHub URL**”。可选地，您可以指定一个自定义子域。在下面的示例中，该应用程序将部署到 `https://red-balloon.streamlit.app/`。您始终可以在以后设置或更改您的子域。有关更多关于[自定义子域](#custom-subdomains)的信息，请参阅本页面末尾。

![部署应用程序](/images/streamlit-community-cloud/deploy-an-app.png)

## 部署的高级设置

如果您要连接到数据源或者想为您的应用程序选择一个Python版本，您可以在部署应用程序之前点击 "**高级设置**"。

![高级设置](/images/streamlit-community-cloud/advanced-settings.png)

您可以使用[秘密管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)来连接私有数据源。详细了解如何[连接数据源](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources)。

<Tip>

Streamlit Community Cloud支持Python 3.7 - Python 3.11，并默认使用版本3.9。您可以在"高级设置"对话框中的"Python版本"下拉菜单中选择您喜欢的版本。

</Tip>

## 观察应用程序启动

您的应用程序正在部署中，您可以观察它的启动过程。大多数应用程序只需要几分钟就可以部署完成，但如果您的应用程序有很多依赖项，第一次部署可能需要一些时间。在初始部署之后，任何不涉及依赖项的更改都应该立即显示出来。

![观察应用程序启动](/images/streamlit-community-cloud/watch-app-launch.png)

<Note>

右侧的云日志只能由开发人员查看，您可以通过它来获取日志并调试应用程序中的任何问题。[了解更多关于云日志的信息](/streamlit-community-cloud/get-started/manage-your-app#cloud-logs)。

</Note>

## 您的应用程序URL

就是这样-您完成了！您的应用程序现在有一个唯一的子域名URL，您可以与他人共享。点击[这里](/streamlit-community-cloud/get-started/share-your-app)阅读如何与观众分享您的应用程序的相关信息。

### 唯一子域名

如果没有设置自定义子域名，应用程序的URL将基于您的GitHub存储库的结构。URL以拥有您的存储库的GitHub用户名或组织名称开头，然后是存储库名称、应用程序路径和一个短哈希值。如果您从除`main`或`master`之外的分支部署，URL还包括分支名称。

```bash
https://[GitHub username or organization]-[repo name]-[app path]-[branch name]-[short hash].streamlit.app
```

例如，这是从`streamlit`组织部署的一个应用程序。仓库名为`demo-self-driving`，根目录中的应用程序名为`streamlit_app.py`。分支名称为`master`，因此未包含在内。

```bash
https://streamlit-demo-self-driving-streamlit-app-8jya0g.streamlit.app
```

### 自定义子域名

默认的子域名不一定是最容易记忆或分享的。因此，您还可以为您的应用设置一个自定义域名。URL将显示为：

```bash
https://<your-custom-subdomain>.streamlit.app
```

要在仪表板中查看或自定义应用程序子域名，请按照以下步骤操作：

1. 单击应用程序右侧的 "**︙**" 溢出菜单，并选择 "**Settings**"。

   ![自定义子域名设置](/images/streamlit-community-cloud/custom-subdomain-settings.png)

2. 在应用程序设置模态窗口的 "**General**" 选项卡中查看。您的应用程序的唯一子域名将显示在此处。
   ![选择自定义子域名](/images/streamlit-community-cloud/custom-subdomain-pick.png)

3. 选择一个自定义子域名，长度在6到63个字符之间，作为应用程序的URL，并点击“**保存**”。
   ![自定义子域名保存](/images/streamlit-community-cloud/custom-subdomain-save.png)

就是这么简单！您可以通过访问您的自定义子域名URL来访问您的应用程序🎉。

如果自定义子域名不可用（例如，已经被占用），您将看到如下错误消息：

<Image src="/images/streamlit-community-cloud/custom-subdomain-error.png" clean />

### 嵌入应用程序

<Tip>

嵌入应用程序的文档已经移动到[Embed your app](/streamlit-community-cloud/get-started/embed-your-app)。请更新您的书签。

</Tip>