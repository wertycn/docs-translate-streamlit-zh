---
slug: /streamlit-community-cloud/get-started
title: Get started
---

# 入门指南

欢迎来到Streamlit社区云！在开始使用Streamlit社区云之前，首先您需要有一个要部署的Streamlit应用程序。如果您还没有构建一个应用程序，请阅读我们的[入门指南](/library/get-started)文档或从[示例应用程序](https://streamlit-cloud-example-apps-streamlit-app-sw3u0r.streamlit.app/)开始。无论哪种方式，创建您的第一个应用程序只需要几分钟时间。

## Streamlit社区云的工作原理

Streamlit社区云是一个供团队部署、管理和协作的工作空间。您可以将Streamlit社区云帐户直接连接到您的GitHub存储库（公共或私有），然后Streamlit社区云将直接从您在GitHub上存储的代码启动应用程序。大多数应用程序只需几分钟即可启动，每当您在GitHub上更新代码时，应用程序将自动更新。这为您部署的应用程序创建了一个快速迭代周期，使开发人员和应用程序的查看者可以快速原型、探索和更新应用程序。

<!-- <Tip>

不使用GitHub？我们正在构建对GitLab、Azure DevOps、Bitbucket和其他提供商的支持。[联系我们的企业团队](https://forms.streamlit.io/cloud-sign-up)获取更多详细信息。

</Tip> -->

在幕后，Streamlit Community Cloud负责容器化、认证、扩展、安全等所有工作，这样你只需要关心应用程序的创建。维护Streamlit应用程序非常容易。容器会获得最新的安全补丁，并且会被主动监控容器的健康状况。我们还正在构建观察和监控应用程序的能力。

## 入门指南

使用Streamlit Community Cloud设置工作环境只需要几分钟。

1. [注册Streamlit Community Cloud账号](#sign-up-for-streamlit-cloud)
2. [登录到您的账号](#log-in-to-sharestreamlitio)
3. [将您的Streamlit Community Cloud账号连接到GitHub](#connect-your-github-account)
4. [探索您的Streamlit Community Cloud工作空间](#explore-your-streamlit-cloud-workspace)
5. [邀请其他开发者加入您的团队](#invite-other-developers-to-your-workspace)

## 注册Streamlit Community Cloud账号

Streamlit的社区云允许您免费使用Streamlit直接部署、管理和共享您的应用程序。请在[Community Cloud主页](https://streamlit.io/cloud)上注册。

注册成功后，请登录[share.streamlit.io](https://share.streamlit.io)，然后按照以下步骤操作。

## 登录share.streamlit.io

您可以使用以下方式登录Streamlit社区云：

1. [Google](#sign-in-with-google)
2. [GitHub](#sign-in-with-github)
3. [电子邮件](#使用电子邮件登录)登录链接：这些链接是一次性的，有效期为15分钟。

<!-- 如果您是开发人员，我们建议您在首次登录时使用GitHub。您可以稍后设置您的帐户以使用Google或一个[SSO提供者](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)进行登录。 -->

如果您是开发人员，我们建议您在第一次登录时使用GitHub进行登录。您稍后可以设置您的账户以使用Google登录。

如果您正在共享您的应用程序，您的应用程序用户可以使用上述任何一种方法进行登录。

<div style={{ maxWidth: '50%', marginBottom: '-2em', marginLeft: '10em' }}>
    <Image alt="Cloud sign in" src="/images/streamlit-community-cloud/cloud-sign-in.png" clean />
</div>

<!-- <Note>

Streamlit Community Cloud提供对所有其他单点登录（SSO）提供商的支持，但您需要在[企业计划](https://forms.streamlit.io/cloud-sign-up)上才能连接SSO。

### 使用Google账号登录

访问[share.streamlit.io](https://share.streamlit.io)，然后点击"Continue with Google"按钮。

![步骤1：点击“Continue with Google”按钮](/images/streamlit-community-cloud/google-signin-1.png)

在下一页上，选择一个帐户进行登录，并输入您的Google帐户凭据。

![Step 2: 输入您的Google帐户凭据](/images/streamlit-community-cloud/google-signin-2.png)

一旦您成功登录Google，您将进入Streamlit社区云工作区！🎈

![您的Streamlit社区云工作区](/images/streamlit-community-cloud/app-workspace.png)

### 使用GitHub登录

访问[share.streamlit.io](https://share.streamlit.io)，然后点击"Continue with GitHub"按钮。

![Step 1: 点击'Continue with GitHub'按钮](/images/streamlit-community-cloud/github-signin-1.png)

在接下来的页面中，输入您的GitHub凭据进行登录。

![Step 2: 输入您的GitHub账户凭据](/images/streamlit-community-cloud/github-signin-2.png)

一旦您登录到GitHub，您将进入Streamlit社区云工作空间！🎈

<Image caption="您的Streamlit社区云工作空间" src="/images/streamlit-community-cloud/app-workspace.png" />

### 使用电子邮件登录

如果您没有SSO，您可以使用您的电子邮件地址登录！访问[share.streamlit.io](https://share.streamlit.io)，输入您用于注册Streamlit社区云的电子邮件地址，并单击“继续使用电子邮件”按钮。

![Step 1: 输入您的电子邮件地址并点击“继续使用电子邮件”](/images/streamlit-community-cloud/email-signin-1.png)

完成后，您将看到一个确认消息（如下所示），要求您检查您的电子邮件。

![Step 2: 在收件箱中查看来自Streamlit的电子邮件](/images/streamlit-community-cloud/email-signin-2.png)

请检查您的收件箱，查找来自Streamlit的邮件，主题为“登录到Streamlit社区云”。点击邮件中的链接以登录到Streamlit。请注意，该链接将在15分钟后过期，并且只能使用一次。

<Image caption="第三步：点击邮件中的链接以登录到Streamlit" src="/images/streamlit-community-cloud/email-signin-3.png" />

一旦您点击了邮件中的链接，您将进入Streamlit社区云工作空间！🎈

![Your Streamlit Community Cloud workspace](/images/streamlit-community-cloud/app-workspace.png)

## 连接您的 GitHub 账户

接下来，您需要授权Streamlit连接到您的GitHub帐户。这将允许Streamlit Community Cloud工作空间直接从您存储在存储库中的应用程序文件中启动应用程序，并且可以检查这些应用程序文件的更新，以便您的应用程序可以自动更新。您将看到两个不同的授权屏幕来给予此访问权限。请在两个屏幕上都点击"授权"。关于GitHub权限的问题？[在此处阅读更多](/streamlit-community-cloud/troubleshooting#github-integration)！

<重要>

您需要在您的存储库中拥有**管理员**权限才能部署应用程序。如果您没有管理员访问权限，请与您的IT团队或经理联系，帮助您设置Streamlit Community Cloud账户，或在[社区论坛](https://discuss.streamlit.io/)上联系我们。

</重要>

<div style={{ marginBottom: '-3em' }}>
    <Flex>
    <Image caption="授权屏幕1" src="/images/streamlit-community-cloud/authorization-1.png" />
    <Image caption="授权界面2" src="/images/streamlit-community-cloud/authorization-2.png" />
    </Flex>
</div>

<Note>

一旦用户被添加到GitHub存储库中，最多需要15分钟才能在云上部署应用程序。如果用户从GitHub存储库中删除，最多需要15分钟才能撤销其对该存储库的应用程序管理权限。

</Note>

## 探索您的Streamlit Community Cloud工作空间

恭喜！您已成功登录并准备好开始了。如果您加入了其他人的工作区，您可能已经在工作区中看到了应用程序。如果没有，那么您需要部署一个应用程序！请查看我们下一节关于如何[部署应用程序](/streamlit-community-cloud/get-started/deploy-an-app)的内容。如果您需要一个要部署的应用程序，请查看我们的[示例应用程序](https://streamlit-cloud-example-apps-streamlit-app-sw3u0r.streamlit.app/)，其中包括机器学习、数据科学和业务用例的应用程序。

![Workspace 1](/images/streamlit-community-cloud/workspace-1.png)

您可能会发现您已经有多个Streamlit Community Cloud工作区。Streamlit Community Cloud会根据相应的GitHub存储库的所有者自动将您的应用程序分组。在右上角，您可以看到您可以访问的工作区。如果您的团队已经启动了应用程序，那么您将在您的工作区中看到这些应用程序。在[这里](/streamlit-community-cloud/get-started/manage-your-app#app-workspaces)了解更多关于工作区的信息。

![Workspace 2](/images/streamlit-community-cloud/workspace-2.png)

## 邀请其他开发者加入您的工作区

邀请其他开发者非常简单，只需邀请他们加入您的GitHub仓库，这样您可以一起进行应用程序的编码，然后让他们登录到[share.streamlit.io](https://share.streamlit.io)。如果您正在团队中工作，可能已经在同一个仓库中，所以跳过步骤1，直接让他们登录到[share.streamlit.io](https://share.streamlit.io)。

Streamlit Community Cloud继承了GitHub的开发者权限，因此当您的团队成员登录时，他们将自动查看您共享的工作区。从那里，您可以一起部署、管理和共享应用程序。

请记住，只要团队中的任何人在GitHub上更新了代码，应用程序也会自动更新给您！