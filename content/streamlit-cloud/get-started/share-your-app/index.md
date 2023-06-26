---
title: 分享你的应用程序
slug: /streamlit-community-cloud/get-started/share-your-app
---

# 分享你的应用程序

现在你的应用程序已经部署好了，你可以轻松地分享和协作。但首先，让我们花点时间为成功部署应用程序而跳一支小舞吧！🕺💃

您的应用现在已经在固定的URL上上线了，所以您可以尽情地与任何人分享。您的应用将继承您的GitHub存储库的权限，这意味着如果您的存储库是私有的，您的应用将是私有的，如果您的存储库是公开的，您的应用将是公开的。如果您想更改这一点，您可以直接从应用菜单中进行操作。

- [分享私有应用](#sharing-private-apps)
- [分享公开应用](#sharing-public-apps)
- [添加开发者](/streamlit-community-cloud/get-started/share-your-app#adding-developers)

有三种主要的方法可以与观看者共享您的应用程序。您可以直接从应用程序内的共享菜单中添加观看者，或者从云日志菜单中添加观看者，或者从您的应用程序仪表板中添加观看者。

## 共享私有应用程序

默认情况下，从私有源代码部署的所有应用程序对工作区中的开发人员是私有的。除非您授予他们明确的权限，否则其他人将无法看到您的应用程序。您可以在工作区或应用程序本身中授予权限。

![分享私有应用](/images/streamlit-community-cloud/sharing-private-apps.png)

### 什么是查看者认证（viewer auth）？

<!-- 查看者身份验证功能允许您限制应用程序的查看者。要访问您的应用程序，用户必须使用基于电子邮件的无密码登录或Google OAuth进行身份验证。

### 配置单点登录 -->

<!-- Google OAuth默认启用，因此如果您的公司使用Google，您可以直接使用。如果您通过ADFS、Azure、Okta或通用SAML配置了组织的SSO，您还可以添加由这些服务管理的电子邮件地址和域。阅读[此处](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)了解如何为您的组织启用SSO。 -->

Google OAuth默认启用，因此如果您使用Google，您可以直接使用。

<!--一旦您将某人的电子邮件地址添加到应用程序的查看者列表中，该人将能够通过Google单点登录或您的特定于组织的单点登录来登录并查看您的应用程序。他们还将收到一封邀请他们查看您的应用程序的电子邮件。如果他们已经在他们的浏览器中使用该帐户登录（对于大多数人来说通常是这样），那么他们将自动能够查看应用程序。如果他们没有登录，或者他们没有获得访问权限，则他们将看到一个要求他们首先登录的页面。-->

一旦您将某人的电子邮件地址添加到您的应用程序的查看者列表中，该人将能够通过Google单点登录进行登录并查看您的应用程序。他们还将收到一封邀请他们查看您的应用程序的电子邮件。如果他们已经在他们的浏览器中使用该帐户登录（大多数人的情况），那么他们将自动能够查看应用程序。如果他们未登录，或者他们未获得访问权限，则他们将看到一个页面要求他们先登录。

<提示>

遇到授权问题吗？查看我们的[故障排除部分](/streamlit-community-cloud/troubleshooting)获取帮助。

</Tip>

<!-- ### 授予整个组织访问权限 -->

如果您添加了一个完整的电子邮件域，使用该域的电子邮件地址的任何人在验证身份后将能够查看您的应用程序。例如，如果将"foo.com"添加到允许的电子邮件域列表中，任何以"@foo.com"结尾的电子邮件地址的人都可以查看该应用程序。

有三种主要的方法可以与观众分享您的应用程序。您可以直接从应用内共享菜单中添加观众，也可以从云日志菜单中添加观众，或者从您的应用程序仪表板中添加观众。

### 从应用内共享菜单中添加观众

您可以通过点击部署的应用程序右上角的“分享”按钮来从应用内共享菜单中添加观众。

1. **点击右上角的“分享”按钮。**

<div style={{ marginBottom: '-3em', marginLeft: '2em' }}>
    ![图片](/images/streamlit-community-cloud/in-app-share-menu-1.png)
 
2. **输入查看者的邮箱地址。**

![图片](/images/streamlit-community-cloud/in-app-share-menu-2.png)

3. **点击"邀请"。**

   如此简单！您添加的观众将收到一封邀请他们访问您的应用程序的电子邮件。最近添加的观众将显示在应用内共享菜单的列表顶部。

<div style={{ maxWidth: '75%', marginBottom: '-1em', marginLeft: '4em' }}>
    <Image src="/images/streamlit-community-cloud/app-invite-notification.png" />
</div>

要移除一个观众，只需将鼠标悬停在其电子邮件地址上，然后点击出现在右侧的"X"按钮。

<div style={{ maxWidth: '55%', marginBottom: '-1em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/in-app-share-menu-3.png" />
</div>

开发者、受邀观众以及工作空间成员都可以看到应用内的共享菜单，阅读观众列表，添加和移除观众。

<Important>

只有开发者有权限切换应用程序的公开或私有设置。应用程序观众没有权限更改此设置。

</Important>

### 从云日志菜单中添加查看者

您可以从部署的应用程序中轻松地从云日志菜单中添加查看者。

1. **在右下角选择"管理应用程序"。**

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/manage-app.png" />
</div>

2. **从菜单中选择"设置"。**

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <div>
   <Image src="/images/streamlit-community-cloud/settings-menu.png" />
</div>

3. **在设置中添加查看者。**

   您可以选择仅允许基于其个人电子邮件的特定查看者。请确保将它们输入为逐行分隔的列表。

   ![添加查看者](/images/streamlit-community-cloud/add-viewers.png)

### 从应用程序仪表盘中添加查看者

您还可以直接从仪表盘中添加查看者。

1. **打开应用程序的设置**

   导航到您想要添加查看器的应用程序，并单击汉堡图标选择“设置”。

<div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
   <Image src="/images/streamlit-community-cloud/edit-secrets.png" />
</div>

2. **在设置中添加查看器**

在应用程序设置中的“共享”部分中，点击文本输入区域，在其中提供一个以换行符分隔的电子邮件地址列表，用于将查看器访问权限授予您的应用程序的用户。点击“保存”。

   <div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
       <Image src="/images/streamlit-community-cloud/add-viewers.png" />
   </div>

## 共享公共应用程序

从您部署的应用程序中，您可以点击右上角的"☰"菜单，然后选择'分享此应用程序'，直接将其发布到社交媒体或与我们的[社区论坛](https://discuss.streamlit.io/c/streamlit-examples/9)共享。我们很乐意看到您的作品，并可能将您的应用程序作为我们的每月应用程序特色展示 ❤️。

### 添加GitHub徽章

为了帮助其他人找到并使用您的Streamlit应用程序，您可以将Streamlit的GitHub徽章添加到您的仓库中。下面是徽章的示例。点击徽章会带您到Streamlit的Face-GAN演示页面。

<div style={{ marginBottom: '-2em', marginLeft: '30%' }}>
    <a href="https://streamlit-demo-face-gan-streamlit-app-v2nxgz.streamlit.app/" target="_blank" style={{ borderBottom: 0 }}>
    <div>
    <a href="https://static.streamlit.io/badges/streamlit_badge_black_white.svg">
        <img src="https://static.streamlit.io/badges/streamlit_badge_black_white.svg" />
    </a>
</div>

一旦您部署了您的应用程序，您可以通过添加以下的Markdown将此徽章嵌入到您的GitHub README.md中。

```markdown
[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://<your-custom-subdomain>.streamlit.app)
```

<注意>

请确保将 `https://<your-custom-subdomain>.streamlit.app` 替换为您部署应用程序的URL！

</注意>

## 添加开发者

邀请其他开发者非常简单，只需邀请他们加入您的GitHub代码库，这样您就可以一起进行应用程序的编码，然后让他们登录到[share.streamlit.io](https://share.streamlit.io)。如果您正在团队中工作，很可能已经在同一个代码库中，所以跳过步骤1，直接让他们登录到[share.streamlit.io](https://share.streamlit.io)。

Streamlit社区云从GitHub继承开发者权限，因此当您的团队成员登录时，他们将自动查看您共享的工作空间。从那里，您可以一起部署、管理和共享应用程序。

### 推送新代码

您还可以与其他开发者合作，让多个贡献者推送到同一个GitHub仓库。无论团队中的任何人在GitHub上更新代码，应用程序也会自动更新给您！

如果您想尝试一些新的东西，同时保持原始应用程序的运行，只需创建一个新的分支，进行一些更改，并部署Streamlit应用程序的新版本。

### 查找应用程序代码

每个部署的应用程序都在右上角的"☰"菜单中链接了其GitHub源代码。因此，如果您想了解另一个Streamlit应用程序的代码，您可以从那里导航到GitHub页面并阅读或复制该应用程序。
