---
slug: /streamlit-community-cloud/get-started/deploy-an-app
title: Deploy an app
---

# 部署应用程序

Streamlit社区云允许您通过仅点击一次即可部署您的应用程序，大多数应用程序只需要几分钟即可部署。如果您还没有准备好部署的应用程序，[可以fork或克隆我们的示例应用程序之一](https://streamlit-cloud-example-apps-streamlit-app-sw3u0r.streamlit.app/?hsCtaTracking=28f10086-a3a5-4ea8-9403-f3d52bf26184|22470002-acb1-4d93-8286-00ee4f8a46fb) — 您可以找到用于机器学习、数据可视化、数据探索、A/B测试等的应用程序。

<Note>

如果您想在不同的云服务上部署您的应用程序，请查看我们的知识库中的[部署 Streamlit 应用程序](/knowledge-base/tutorials/deploy)文章。

</Note>

## 将您的应用程序添加到 GitHub

Streamlit Community Cloud 会直接从您的 GitHub 存储库中启动应用程序，因此在尝试部署应用程序之前，您的应用程序代码和依赖项需要存储在 GitHub 上。有关更多信息，请参阅[应用程序依赖关系](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies)。

### 可选：添加配置文件

Streamlit允许您通过四种不同的方法可选地设置配置选项。除此之外，您还可以使用自定义配置来自定义应用程序的主题，启用日志记录或设置应用程序运行的端口。有关更多信息，请参阅[配置](/library/advanced-features/configuration)和[主题化](/library/advanced-features/theming)。然而，在Streamlit Community Cloud上，您只能通过GitHub存储库中的配置文件来设置配置选项。

具体来说，您可以在存储库的根目录（顶级目录）中添加一个配置文件：创建一个名为`.streamlit`的文件夹，然后在该文件夹中添加一个名为`config.toml`的文件。例如，如果您的应用程序存储在名为`my-app`的存储库中，您将添加一个名为`my-app/.streamlit/config.toml`的文件。假设您想将应用程序的主题设置为“暗黑模式”。您可以将以下内容添加到您的`.streamlit/config.toml`文件中：

```toml
[theme]
base="dark"
```

<重要>

无论存储库中有多少个应用程序，只能存在一个配置文件。

</重要>

## 部署您的应用程序

要部署应用程序，请单击工作区右上角的“**新建应用**”。

![新建应用](/images/streamlit-community-cloud/deploy-empty-new-app.png)

填写您的仓库、分支和文件路径。作为快捷方式，您也可以点击“**粘贴GitHub URL**”。可选地，您还可以指定一个自定义子域名。在下面的示例中，该应用将部署到 `https://red-balloon.streamlit.app/`。您随时可以设置或更改您的子域名。有关更多关于[自定义子域名](#custom-subdomains)的信息，请参阅本页面末尾的内容。

![部署应用](/images/streamlit-community-cloud/deploy-an-app.png)

## 高级部署设置

如果您要连接到数据源或者为您的应用程序选择Python版本，您可以在部署应用之前，通过点击 "**高级设置**" 来进行设置。

![高级设置](/images/streamlit-community-cloud/advanced-settings.png)

您可以使用[秘密管理](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)来连接私有数据源。了解更多关于如何[连接数据源](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources)的信息。

<Tip>

Streamlit Community Cloud支持Python 3.7 - Python 3.11，并默认使用版本3.9。您可以在“高级设置”模态框中的“Python版本”下拉菜单中选择您喜欢的版本。

</提示>

## 观察应用程序的启动

您的应用程序正在部署中，您可以观察它的启动过程。大多数应用程序只需要几分钟就可以部署完成，但如果您的应用程序有很多依赖项，第一次部署可能需要一些时间。在初始部署之后，不涉及依赖项的任何更改应该立即显示出来。

![观察应用程序启动](/images/streamlit-community-cloud/watch-app-launch.png)

<注意>

右侧的云日志只能由开发者查看，您可以使用它来获取日志并调试应用程序中的任何问题。[了解更多关于云日志的信息](/streamlit-community-cloud/get-started/manage-your-app#cloud-logs)。

</Note>

## 您的应用程序 URL

就是这样 - 您完成了！您的应用程序现在具有一个唯一的子域名 URL，您可以与他人共享。点击[这里](/streamlit-community-cloud/get-started/share-your-app)了解如何与观看者共享您的应用程序。

### 唯一子域名

如果没有设置自定义子域名，应用程序的URL将根据您的GitHub存储库结构生成。URL以您的GitHub用户名或组织名开头，后跟您的存储库名称、应用程序路径和一个短哈希值。如果您从除`main`或`master`之外的分支部署，URL还包括分支名称。

```bash
https://[GitHub username or organization]-[repo name]-[app path]-[branch name]-[short hash].streamlit.app
```

例如，这是一个从`streamlit`组织部署的应用程序。该仓库名为`demo-self-driving`，根目录中的应用程序名为`streamlit_app.py`。分支名为`master`，因此不包含在内。

```bash
https://streamlit-demo-self-driving-streamlit-app-8jya0g.streamlit.app
```

### 自定义子域名

默认子域名并不总是最易记忆或分享的。因此，您还可以为您的应用设置一个自定义域名。URL将显示为：

```bash
https://<your-custom-subdomain>.streamlit.app
```

在仪表板中查看或自定义应用程序子域的步骤：

1. 点击应用程序右侧的 "**︙**" 溢出菜单，选择 "**设置**"。

   ![自定义子域设置](/images/streamlit-community-cloud/custom-subdomain-settings.png)

2. 在应用程序设置弹出窗口的 "**常规**" 选项卡中，您的应用程序的唯一子域将显示在这里。
   ![选择自定义子域](/images/streamlit-community-cloud/custom-subdomain-pick.png)

3. 选择一个自定义的子域名，长度为6到63个字符，用于您的应用程序的URL，并点击“**保存**”。
   ![自定义子域名保存](/images/streamlit-community-cloud/custom-subdomain-save.png)

就是这么简单！然后，您可以通过访问您的自定义子域名URL来访问您的应用程序 🎉。

如果自定义子域名不可用（例如，因为已经被占用），您将看到如下错误消息：

![自定义子域名错误](/images/streamlit-community-cloud/custom-subdomain-error.png)

### 嵌入应用程序

<Tip>

嵌入应用程序的文档已经移动到[Embed your app](/streamlit-community-cloud/get-started/embed-your-app)。请更新您的书签。

</Tip>