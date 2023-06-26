---
slug: /knowledge-base/deploy/custom-subdomains
title: Custom subdomains
---

# 自定义子域名

一旦您在Community Cloud上部署了您的应用程序，它将被分配一个自动生成的子域名，该子域名基于您的GitHub存储库的结构。这个子域名是唯一的，并且可以用来与他人共享您的应用程序。然而，默认的子域名并不总是最容易记住或共享的。例如，下面的子域名有点拗口！

`https://streamlit-demo-self-driving-streamlit-app-8jya0g.streamlit.app`

您还可以设置一个自定义子域名，以便更容易分享您的应用程序。您可以自定义子域名以反映您的应用程序内容、个人品牌或其他任何您喜欢的内容。URL将显示为：

```
<your-custom-subdomain>.streamlit.app
```

要从[仪表板](/streamlit-community-cloud/get-started/manage-your-app#manage-apps-from-your-app-dashboard)自定义应用子域名，请按以下步骤操作：

1. 单击应用程序右侧的"︙"溢出菜单，然后选择"**设置**"。

   ![自定义子域名设置](/images/streamlit-community-cloud/custom-subdomain-settings.png)

2. 在应用设置模态窗口的"**常规**"选项卡中查看。您的应用程序的唯一子域名将显示在此处。
   ![自定义子域名选择](/images/streamlit-community-cloud/custom-subdomain-pick.png)

3. 选择一个长度为6到63个字符的自定义子域名作为您的应用程序的URL，并点击“**保存**”。
   ![自定义子域名保存](/images/streamlit-community-cloud/custom-subdomain-save.png)

就是这么简单！您可以通过访问您的自定义子域名URL来访问您的应用程序🎉。

如果自定义子域名不可用（例如，因为已被占用），您将看到如下的错误消息:

![自定义子域名错误](/images/streamlit-community-cloud/custom-subdomain-error.png)