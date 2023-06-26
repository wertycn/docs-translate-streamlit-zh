---
slug: /knowledge-base/deploy/share-apps-with-viewers-outside-organization
title: How do I share apps with viewers outside my organization?
---

# 如何与组织外的观众共享应用程序？

当您与组织外的人分享应用程序时，他们将收到一封邀请他们使用他们的电子邮件登录的电子邮件。

<提示>

点击[这里](/streamlit-community-cloud/get-started#sign-in-with-email)了解更多关于[使用电子邮件登录](/streamlit-community-cloud/get-started#sign-in-with-email)的信息。

</提示>

查看器身份验证允许您限制应用程序的查看者。为了访问您的应用程序，用户必须使用基于电子邮件的无密码登录或[单点登录（SSO）](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)进行身份验证。您可以通过以下两种方式与组织外的查看者分享您的应用程序：

- [从应用程序中添加查看者](#adding-viewers-from-the-app)
- [从您的仪表板中添加查看者](#adding-viewers-from-your-dashboard)

## 从应用程序中添加查看器

您可以通过开发者控制台轻松地从部署的应用程序中添加查看器。

1. **在右下角选择"管理应用程序"。**
<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/manage-app.png" />
</div>
2. **从菜单中选择"设置"。**
<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <div>
3. **在设置中添加查看者。**

   您可以选择仅允许基于其个人电子邮件的特定查看者。请确保将它们输入为逐行分隔的列表。

   ![添加查看者](/images/streamlit-community-cloud/add-viewers.png)

您添加的查看者将收到一封电子邮件，类似于下面的邀请他们使用[电子邮件登录](/streamlit-community-cloud/get-started#sign-in-with-email)的邮件。

<img src="/images/streamlit-community-cloud/app-invite-notification.png" />

## 从仪表板中添加查看者

您还可以直接从仪表板中添加查看者。

1. **打开应用程序的设置**

   导航到您想要添加查看者的应用程序，并点击汉堡图标选择“设置”。

   <div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
       <img src="/images/streamlit-community-cloud/edit-secrets.png" />
   </div>

2. **在设置中添加查看者**

   在应用程序设置中点击"共享"部分，在文本输入区域中，提供一个以换行符分隔的电子邮件地址列表，以授予用户对您的应用程序的查看权限。点击"保存"。

   <div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
       <Image src="/images/streamlit-community-cloud/add-viewers.png" />
   </div>

您添加的观众将收到一封电子邮件，类似下面的邮件，邀请他们使用[电子邮件进行登录](/streamlit-community-cloud/get-started#sign-in-with-email)。

<Image src="/images/streamlit-community-cloud/app-invite-notification.png" />