---
slug: /streamlit-community-cloud/get-started/share-your-app
title: Share your app
---

# 分享您的应用程序

现在，您可以轻松分享和协作您的应用程序。但首先，让我们花点时间为成功部署应用程序而狂欢一下！🕺💃

您的应用程序现在可以通过固定的URL访问，因此您可以随心所欲地与任何人分享。您的应用程序将继承您的GitHub存储库的权限，这意味着如果您的存储库是私有的，您的应用程序也将是私有的；如果您的存储库是公开的，您的应用程序也将是公开的。如果您想要更改这一点，您可以在应用程序菜单中轻松进行调整。

- [分享私有应用程序](#sharing-private-apps)
- [分享公开应用程序](#sharing-public-apps)
- [添加开发者](/streamlit-community-cloud/get-started/share-your-app#adding-developers)

有三种主要的方式来与观看者分享您的应用程序。您可以直接从应用内的共享菜单添加观看者，或者从云日志菜单或应用程序仪表板中添加观看者。

## 分享私有应用程序

默认情况下，从私有源代码部署的所有应用程序对工作区中的开发人员都是私有的。除非您明确授予他们权限，否则其他人将无法看到您的应用程序。您可以在工作区中或应用程序本身授予权限。

![共享私有应用程序](/images/streamlit-community-cloud/sharing-private-apps.png)

### 什么是查看者授权？

<!-- 查看者身份验证允许您限制应用程序的查看者。要访问您的应用程序，用户必须使用基于电子邮件的无密码登录或Google OAuth进行身份验证。

### 配置单点登录 -->

<!-- Google OAuth默认启用，所以如果你的公司使用Google，你可以直接进行操作。如果你通过ADFS、Azure、Okta或通用SAML配置了组织的SSO，你也可以添加由这些服务管理的电子邮件地址和域名。点击[这里](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)了解如何为您的组织启用SSO。 -->

Google OAuth默认启用，所以如果你使用Google，你可以直接进行操作。

<!-- 一旦您将某人的电子邮件地址添加到您的应用程序的查看者列表中，该人将能够通过Google单点登录或您的组织特定的单点登录来登录并查看您的应用程序。他们还将收到一封邀请他们查看您的应用程序的电子邮件。如果他们已经在他们的浏览器中使用该帐户登录（大多数人的情况），那么他们将自动能够查看该应用程序。如果他们没有登录，或者他们没有得到访问权限，则他们将看到一个页面要求他们先登录。 -->

一旦您将某人的电子邮件地址添加到您的应用程序的查看者列表中，该人将能够通过Google单点登录来登录并查看您的应用程序。他们还将收到一封邀请他们查看您的应用程序的电子邮件。如果他们已经在浏览器中使用该帐户登录（大多数人的常规情况），那么他们将自动能够查看应用程序。如果他们没有登录或者没有被授权访问，则会看到一个页面要求他们首先登录。

<Tip>提示：</Tip>

遇到授权问题？查看我们的[故障排除部分](/streamlit-community-cloud/troubleshooting)以获取帮助。

</Tip>

<!-- ### 授予整个组织访问权限 -->

如果您添加了一个完整的电子邮件域，那么使用该域的电子邮件地址的任何人在通过身份验证后将能够查看您的应用程序。例如，如果将"foo.com"添加到允许的电子邮件域列表中，那么以"@foo.com"结尾的电子邮件地址的任何人都将被允许查看该应用程序。

有三种主要的方法可以与观众分享您的应用程序。您可以直接从应用程序内的共享菜单中添加观众，也可以从云日志菜单或应用程序仪表板中添加观众。

### 从应用程序内的共享菜单中添加观众

您可以通过点击已部署应用程序右上角的"分享"按钮来从应用程序内的共享菜单中添加观众。

1. **点击右上角的"分享"按钮。**

<div style={{ marginBottom: '-3em', marginLeft: '2em' }}>
    <img src="/images/streamlit-community-cloud/in-app-share-menu-1.png" />

2. **输入查看者的邮箱地址。**

<img src="/images/streamlit-community-cloud/in-app-share-menu-2.png" />

3. **点击“邀请”。**

   如此简单！您添加的观众将收到一封邀请邮件，邀请他们访问您的应用程序。最近添加的观众将出现在应用内共享菜单的顶部。

<div style={{ maxWidth: '75%', marginBottom: '-1em', marginLeft: '4em' }}>
    <Image src="/images/streamlit-community-cloud/app-invite-notification.png" />
</div>

要删除观众，只需将鼠标悬停在其电子邮件地址上，然后单击出现在右侧的“X”按钮。

<div style={{ maxWidth: '55%', marginBottom: '-1em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/in-app-share-menu-3.png" />
</div>

开发者、被邀请的观众以及工作区的成员都可以看到应用内的共享菜单，查看观众列表，并添加和移除观众。

<Important>

只有开发者有权切换应用程序的公开或私有状态。应用程序的观众没有权限更改此设置。

</Important>

### 从云日志菜单添加查看器

您可以从部署的应用程序中轻松地从云日志菜单中添加查看器。

1. **在右下角选择"管理应用程序"。**

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/manage-app.png" />
</div>

2. **从菜单中选择"设置"。**

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <div>

3. **在设置中添加查看者。**

   您可以选择仅允许基于其个人电子邮件的选定查看者。请确保将它们作为分行列表输入。

   ![添加查看者](/images/streamlit-community-cloud/add-viewers.png)

### 从应用程序仪表板添加查看者

您还可以直接从仪表板中添加查看者。

1. **打开应用程序的设置**

   导航到您想要添加查看器的应用程序，并单击汉堡图标选择“设置”。

   <div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
       <Image src="/images/streamlit-community-cloud/edit-secrets.png" />
   </div>

2. **在设置中添加查看器**

   在应用程序设置中点击“共享”部分，在文本输入区域中，提供一个逐行分隔的电子邮件地址列表，用于授予用户对您的应用程序的查看器访问权限。 点击“保存”。

   <div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
       <Image src="/images/streamlit-community-cloud/add-viewers.png" />
   </div>

## 共享公共应用程序

从您部署的应用程序中，您可以点击右上角的"☰"菜单，然后选择“分享此应用程序”将其直接发布到社交媒体，或在我们的[社区论坛](https://discuss.streamlit.io/c/streamlit-examples/9)上与社区分享。我们很乐意看到您的作品，并可能将您的应用程序作为我们本月的精选应用展示 ❤️。

### 添加GitHub徽章

为了帮助其他人找到并使用您的Streamlit应用程序，您可以将Streamlit的GitHub徽章添加到您的仓库中。下面是徽章的示例。单击徽章可跳转到Streamlit的Face-GAN演示页面。

<div style={{ marginBottom: '-2em', marginLeft: '30%' }}>
    <a href="https://streamlit-demo-face-gan-streamlit-app-v2nxgz.streamlit.app/" target="_blank" style={{ borderBottom: 0 }}>
    <div align="center">
    <a href="https://static.streamlit.io/badges/streamlit_badge_black_white.svg">
        <img src="https://static.streamlit.io/badges/streamlit_badge_black_white.svg" />
    </a>
</div>

一旦您部署了应用程序，您可以通过添加以下Markdown将此徽章嵌入到您的GitHub README.md文件中：

```markdown
[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://<your-custom-subdomain>.streamlit.app)
```

<Note>

请确保将 `https://<your-custom-subdomain>.streamlit.app` 替换为您部署应用程序的URL！

</Note>

## 添加开发人员

邀请其他开发者非常简单，只需邀请他们加入您的GitHub存储库，以便可以一起进行应用程序的编码，然后让他们登录[share.streamlit.io](https://share.streamlit.io)。如果您是作为一个团队工作，可能已经在同一个存储库中，所以可以跳过第一步，直接让他们登录[share.streamlit.io](https://share.streamlit.io)。

Streamlit社区云继承了GitHub的开发者权限，因此当您的团队成员登录时，他们将自动查看您共享的工作区。从那里，您可以一起部署、管理和共享应用程序。

### 推送新代码

您还可以与其他开发者合作，让多个贡献者向同一个GitHub仓库推送代码。每当团队中的任何人更新GitHub上的代码时，应用程序也会自动更新给您！

如果您想尝试一些新的东西，同时保持原始应用程序的运行，只需创建一个新的分支，进行一些更改，并部署Streamlit应用程序的新版本。

### 查找应用程序代码

每个部署的应用程序都在右上角的"☰"菜单中链接了其GitHub源代码。因此，如果您想了解另一个Streamlit应用程序的代码，您可以从那里导航到GitHub页面，并阅读或fork该应用程序。