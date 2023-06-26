---
title: 快速开始
slug: /streamlit-community-cloud/get-started
---

# 快速开始

欢迎使用Streamlit Community Cloud！首先，在开始使用Streamlit Community Cloud之前，您需要有一个要部署的Streamlit应用程序。如果您还没有构建应用程序，请阅读我们的[入门指南](/library/get-started)或从[示例应用](https://streamlit-cloud-example-apps-streamlit-app-sw3u0r.streamlit.app/)开始。无论哪种方式，创建第一个应用程序只需要几分钟。

## Streamlit社区云如何工作

Streamlit Community Cloud是一个为团队提供部署、管理和协作Streamlit应用程序的工作空间。您可以将Streamlit Community Cloud帐户直接连接到您在GitHub上存储的代码库（公开或私有），然后Streamlit Community Cloud将直接从GitHub上的代码启动应用程序。大多数应用程序只需几分钟即可启动，每当您在GitHub上更新代码时，您的应用程序将自动为您更新。这为您部署的应用程序创建了一个快速的迭代周期，使开发人员和应用程序的观众能够快速原型、探索和更新应用程序。

<!-- <提示>

不使用GitHub？我们正在为GitLab、Azure DevOps、Bitbucket和其他提供商提供支持。[联系我们的企业团队](https://forms.streamlit.io/cloud-sign-up)获取更多详细信息。

</提示> -->

在幕后，Streamlit社区云处理所有容器化、身份验证、扩展、安全性等问题，这样您只需要关注创建应用程序即可。维护Streamlit应用程序很容易。容器会获得最新的安全补丁，并会定期监控容器的健康状况。我们还正在开发观察和监控应用程序的能力。

## 入门指南

只需要几分钟，您就可以在Streamlit社区云上设置好您的工作空间。

1. [注册 Streamlit Community Cloud](#sign-up-for-streamlit-cloud)
2. [登录到您的账户](#log-in-to-sharestreamlitio)
3. [将您的 Streamlit Community Cloud 账户连接到 GitHub](#connect-your-github-account)
4. [探索您的 Streamlit Community Cloud 工作空间](#explore-your-streamlit-cloud-workspace)
5. [邀请团队中的其他开发者](#invite-other-developers-to-your-workspace)

## 注册 Streamlit Community Cloud

Streamlit的Community Cloud允许您从Streamlit直接部署、管理和共享您的应用程序，并且所有这些都是免费的。在[Community Cloud主页](https://streamlit.io/cloud)上注册。

注册完成后，登录到[share.streamlit.io](https://share.streamlit.io)，然后按照以下步骤操作。

## 登录到share.streamlit.io

您可以使用以下方式登录到Streamlit Community Cloud：

1. [Google](#sign-in-with-google)
2. [GitHub](#sign-in-with-github)
3. [电子邮件](#通过电子邮件登录)登录链接：这些链接是一次性使用的链接，有效期为15分钟。

<!-- 如果您是开发者，我们建议您第一次登录时使用GitHub。您可以稍后设置您的帐户，使用Google或[SSO提供商](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)登录。 -->

如果您是开发人员，我们建议您首次登录时使用GitHub。您可以随后设置您的帐户以使用Google登录。

如果您要共享您的应用程序，您的应用程序用户可以使用上述任何一种方法进行登录。

<div style={{ maxWidth: '50%', marginBottom: '-2em', marginLeft: '10em' }}>
    <Image alt="Cloud sign in" src="/images/streamlit-community-cloud/cloud-sign-in.png" clean />
</div>

<!-- <Note>

Streamlit Community Cloud支持所有其他单点登录（SSO）提供商，但您需要使用[企业计划](https://forms.streamlit.io/cloud-sign-up)才能连接SSO。

### 使用Google登录

访问[share.streamlit.io](https://share.streamlit.io)，然后点击"使用Google继续"按钮。

![Step 1: 点击'使用Google继续'按钮](/images/streamlit-community-cloud/google-signin-1.png)

在下一页上，选择一个要登录的帐户，并输入您的Google帐户凭据。

![Step 2: 输入您的Google帐户凭据](/images/streamlit-community-cloud/google-signin-2.png)

一旦您登录到Google，您将进入到您的Streamlit Community Cloud工作区！🎈

![您的Streamlit Community Cloud工作区](/images/streamlit-community-cloud/app-workspace.png)

### 使用GitHub登录

访问[share.streamlit.io](https://share.streamlit.io)，然后点击“使用GitHub继续”按钮。

![Step 1: 点击“使用GitHub继续”按钮](/images/streamlit-community-cloud/github-signin-1.png)

在下一页中，输入您的GitHub凭据进行登录。

![Step 2: 输入您的GitHub账号凭据](/images/streamlit-community-cloud/github-signin-2.png)

一旦您登录GitHub，您将进入您的Streamlit Community Cloud工作空间！🎈

![您的Streamlit Community Cloud工作空间](/images/streamlit-community-cloud/app-workspace.png)

### 使用电子邮件登录

如果您没有SSO，您可以使用您的电子邮件地址登录！访问[share.streamlit.io](https://share.streamlit.io)，输入您用于注册Streamlit Community Cloud的电子邮件地址，然后点击"Continue with email"按钮。

<Image caption="步骤 1: 输入您的电子邮件地址并点击 '继续使用电子邮件'" src="/images/streamlit-community-cloud/email-signin-1.png" />

完成后，您将看到一个确认消息（如下所示），要求您检查您的电子邮件。

<Image caption="步骤 2: 在收件箱中查找来自 Streamlit 的电子邮件" src="/images/streamlit-community-cloud/email-signin-2.png" />

请查看您的收件箱，从Streamlit收到了一封主题为“登录Streamlit社区云”的电子邮件。点击邮件中的链接以登录Streamlit。请注意，该链接将在15分钟后过期，并且只能使用一次。

<Image caption="步骤3：点击邮件中的链接以登录Streamlit" src="/images/streamlit-community-cloud/email-signin-3.png" />

一旦您点击了邮件中的链接，您将进入到您的Streamlit社区云工作空间！🎈

<img caption="您的Streamlit社区云工作空间" src="/images/streamlit-community-cloud/app-workspace.png" />

## 连接您的GitHub账户

接下来，您需要授权Streamlit连接到您的GitHub账户。这样，您的Streamlit Community Cloud工作区就可以直接从您存储在仓库中的应用程序文件中启动应用程序，并且系统可以检查这些应用程序文件的更新，以便您的应用程序可以自动更新。您将看到两个不同的授权屏幕来授予这个访问权限。在两个屏幕上都点击"authorize"。关于GitHub权限的问题？[在这里阅读更多](/streamlit-community-cloud/troubleshooting#github-integration)!

<重要>

您必须拥有对存储库的**管理员**权限才能部署应用程序。如果您没有管理员访问权限，请与您的IT团队或经理联系，帮助您设置Streamlit Community Cloud帐户，或在[社区论坛](https://discuss.streamlit.io/)上与我们联系。

</重要>

<div style={{ marginBottom: '-3em' }}>
    <Flex>
    <Image caption="授权屏幕 1" src="/images/streamlit-community-cloud/authorization-1.png" />
    <Image caption="授权屏幕2" src="/images/streamlit-community-cloud/authorization-2.png" />
    </Flex>
</div>

<Note>

一旦用户被添加到GitHub上的存储库中，他们最多需要15分钟才能在云上部署应用程序。如果用户从GitHub上的存储库中移除，他们管理该应用程序的权限将在最多15分钟内被撤销。

</Note>

## 探索您的Streamlit社区云工作空间

恭喜！您已成功登录，准备好开始了。如果您加入了其他人的工作区，您可能已经在工作区中看到了一些应用程序。如果没有，您需要部署一个应用程序！请查看我们下一篇关于[如何部署应用程序](/streamlit-community-cloud/get-started/deploy-an-app)的文章。如果您需要一个要部署的应用程序，请查看我们的[示例应用程序](https://streamlit-cloud-example-apps-streamlit-app-sw3u0r.streamlit.app/)，其中包括机器学习、数据科学和业务用例的应用程序。

![Workspace 1](/images/streamlit-community-cloud/workspace-1.png)

您可能还会发现您已经有多个Streamlit社区云工作区。Streamlit社区云会根据相应的GitHub代码库的所有者自动将您的应用程序进行分组。在右上角您可以看到您有权限访问的工作区。如果您的团队已经启动了应用程序，那么您将在您的工作区中看到这些应用程序。了解有关工作区的更多信息，请点击[此处](/streamlit-community-cloud/get-started/manage-your-app#app-workspaces)。

![Workspace 2](/images/streamlit-community-cloud/workspace-2.png)

## 邀请其他开发者加入您的工作空间

邀请其他开发者非常简单，只需将他们邀请到您的GitHub存储库中，以便您可以一起编写应用程序，然后让他们登录[share.streamlit.io](https://share.streamlit.io)。如果您正在团队中工作，那么您很可能已经在同一个存储库中，因此跳过第1步，直接让他们登录[share.streamlit.io](https://share.streamlit.io)。

Streamlit Community Cloud会继承GitHub的开发者权限，因此当您的团队成员登录时，他们将自动查看您共享的工作区。从那里，您可以一起部署、管理和共享应用程序。

而且请记住，只要团队中的任何人在GitHub上更新代码，应用程序也会自动为您更新！
