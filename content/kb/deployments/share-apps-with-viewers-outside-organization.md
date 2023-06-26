---
title: 如何与组织外的观众共享应用程序？
slug: /knowledge-base/deploy/share-apps-with-viewers-outside-organization
---

# 如何与组织外的观众共享应用程序？

当您与组织外的人共享应用程序时，他们将收到一封电子邮件邀请他们使用其电子邮件地址进行登录。

<Tip>

点击[这里](/streamlit-community-cloud/get-started#sign-in-with-email)了解更多关于[通过电子邮件登录](/streamlit-community-cloud/get-started#sign-in-with-email)的工作原理。

</Tip>

查看者认证允许您限制应用程序的查看者。用户必须使用基于电子邮件的无密码登录或[单点登录（SSO）](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)进行身份验证才能访问您的应用程序。您可以通过以下两种方式与组织外的查看者共享您的应用程序：

- [从应用程序中添加查看者](#添加应用程序中的查看者)
- [从您的仪表板中添加查看者](#从仪表板中添加查看者)

## 从应用程序中添加查看器

您可以从开发者控制台轻松地向您的部署应用程序中添加查看器。

1. **在右下角选择"管理应用程序"。**
<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/manage-app.png" />
</div>
2. **从菜单中选择"设置"。**
<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/settings-menu.png" />
</div>
3. **在设置中添加查看者。**

   您可以选择仅允许特定的查看者根据其个人电子邮件。请确保将它们作为每行一个的列表输入。

   ![添加查看者](/images/streamlit-community-cloud/add-viewers.png)

您添加的查看者将会收到以下类似的电子邮件，邀请他们[使用电子邮件登录](/streamlit-community-cloud/get-started#sign-in-with-email)。

![添加观众](/images/streamlit-community-cloud/app-invite-notification.png)

## 从仪表板中添加观众

您还可以直接从仪表板中添加观众。

1. **打开应用的设置**

   导航到您要添加观众的应用，并点击汉堡图标选择“设置”。

   <div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
       ![编辑秘钥](/images/streamlit-community-cloud/edit-secrets.png)
   </div>

2. **在设置中添加查看者**

   在应用程序设置中点击“共享”部分，在文本输入区域中提供一个以换行分隔的电子邮件地址列表，以授予您希望访问您的应用程序的用户查看权限。点击“保存”。

   <div style={{ maxWidth: '75%', marginBottom: '-3em', marginLeft: '5em' }}>
       <Image src="/images/streamlit-community-cloud/add-viewers.png" />
   </div>

您添加的观众将收到一封电子邮件，类似于下面的内容，邀请他们通过电子邮件进行[登录](/streamlit-community-cloud/get-started#sign-in-with-email)。

![应用邀请通知](/images/streamlit-community-cloud/app-invite-notification.png)
