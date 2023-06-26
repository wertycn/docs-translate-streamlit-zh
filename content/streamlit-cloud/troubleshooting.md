---
slug: /streamlit-community-cloud/troubleshooting
title: Troubleshooting
---

# 故障排除

很抱歉听到您遇到了问题！请查看下面的一些常见问题和解决方法。如果您找不到解决方案，请在我们的[社区论坛](http://discuss.streamlit.io)上发布问题，这样我们的工程师或社区成员可以帮助您。

## 目录

1. [常见问题](#general-help)
2. [应用部署](#deploying-apps)
3. [分享和访问应用](/streamlit-community-cloud/troubleshooting#sharing-and-accessing-apps)
4. [数据和应用程序安全](/streamlit-community-cloud/troubleshooting#data-and-app-security)
<!-- 5. [计费和管理](/streamlit-community-cloud/troubleshooting#billing-and-administration) -->
5. [GitHub集成](/streamlit-community-cloud/troubleshooting#github-integration)
6. [限制和已知问题](/streamlit-community-cloud/troubleshooting#limitations-and-known-issues)

## 一般帮助

### 我如何获取关于我的应用程序的帮助？

如果您有任何问题、反馈、遇到任何问题或需要与我们联系，您可以在我们的[社区论坛](https://discuss.streamlit.io/)上提问。这是最适合与开源库和社区云相关的问题的地方——调试代码、部署、资源限制等。

<!--- 在论坛上提问 ⬅️ 这是最适合与开源库相关的问题的地方
- 您可以通过发送电子邮件至[support@streamlit.io](mailto:support@streamlit.io)联系我们，这是与应用平台相关的任何事情的最佳方式 - 部署、SSO等。

如果您希望，我们也很乐意帮助您审查应用程序的开发。如果您想共享一些代码或让我们的团队查看您的应用程序并给您一些建议，请给我们留言。

## 部署应用程序

### 我的存储库在部署页面上没有显示

可能只是没有显示出来，尽管它已经存在。请尝试手动输入。如果我们无法识别它，你将会看到下面的消息，其中包含一个链接，点击该链接以获取访问权限。

![部署页面故障排除](/images/streamlit-community-cloud/troubleshooting-deploy-page.png)

如果出现任何问题，请尝试退出登录，然后重新登录以确保更改生效。如果仍然无法解决问题，请告诉我们，我们会帮助您解决！

### 无法部署应用程序

首次部署应用程序时，您必须具有对GitHub存储库的管理员级别访问权限。请与管理员确认您是否具有此访问权限。如果没有，请要求管理员首次部署应用程序（我们需要此操作以建立持续集成的Webhooks），然后您可以推送更新到应用程序。

### 我需要为我的应用程序设置特定的Python版本

在部署应用程序时，在高级设置中，您可以选择希望应用程序使用的Python版本。

![Streamlit Community Cloud 高级设置](/images/streamlit-community-cloud/advanced-settings.png)

### 如何在本地存储文件？

如果您想将数据存储在本地而不是数据库中，您可以将文件存储在GitHub存储库中。Streamlit只是Python，所以您可以使用以下方式读取文件：

`pandas.read_csv("data.csv")` 或者 `open("data.csv")`

<Tip>

如果您有非常大的或二进制的数据，并且频繁更改，而且Git感觉很慢，您可能需要查看[Git大文件存储（LFS）](https://git-lfs.github.com/)，它是在GitHub中存储大文件的更好方法。您无需对应用程序进行任何更改即可开始使用它。如果您的GitHub仓库使用LFS，它现在将与Streamlit一起“正常工作”。

</提示>

### 我的应用程序在部署过程中遇到问题

通过点击屏幕右下角的"管理应用程序"展开器，查看您的云日志。通常问题是由于未声明依赖项导致的。有关[依赖管理的更多信息](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies)，请参阅此处。

<!-- 如果这不是问题的原因，请将您看到的日志和警告发送给Streamlit联系人，我们将帮助您解决！ -->

如果这不是问题的原因，请将您看到的日志和警告发送到我们的[社区论坛](https://discuss.streamlit.io/)，我们将帮助您解决！

### 我的应用程序遇到了资源限制/我的应用程序运行非常缓慢

如果您的应用程序运行缓慢或遇到'Argh'页面，我们首先强烈建议您阅读并实施以下博客文章中的建议，以避免应用程序达到资源限制，并检测是否存在内存泄漏问题：

- [常见应用程序问题：资源限制](https://blog.streamlit.io/common-app-problems-resource-limits/)
- [修复应用程序内存泄漏的3个步骤](https://blog.streamlit.io/3-steps-to-fix-app-memory-leaks/)

如果您仍然遇到问题，请单击[此处](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)了解有关资源限制的更多信息。

### 我的应用程序可以获得自定义 URL 吗？

是的！您可以在[这里找到设置自定义子域的说明](/knowledge-base/deploy/custom-subdomains)。

## 共享和访问应用程序

### 我没有 SSO。我如何登录 Streamlit？

没有单点登录(SSO)？没问题！您可以使用电子邮件地址登录Streamlit。点击[这里](/streamlit-community-cloud/get-started#sign-in-with-email)获取有关如何使用电子邮件登录的逐步说明。

<!-- ### How do I add developers to my Streamlit for Teams account? -->

如果您在同一个GitHub仓库中，那么您将自动被添加到同一个工作区。只需邀请他们登录到[share.streamlit.io](http://share.streamlit.io)，一旦他们连接了他们的GitHub账户，我们将自动将他们路由到您的工作区。

### 我如何将观众添加到我的Streamlit应用程序？

<!-- 默认情况下，使用Streamlit Community Cloud Teams和Enterprise部署的所有应用程序都是私有的，这意味着除非您明确授权，否则您的公司中的其他人将无法查看它们。要添加查看者，请使用您组织的SSO提供程序[配置单点登录](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)。

如果您无法实施单点登录，但希望使用密码保护您的Streamlit应用程序，请阅读我们关于[无SSO认证](/knowledge-base/deploy/authentication-without-sso)的指南。注意：尽管此技术增加了一定的安全性，但它**不能**与使用SSO提供程序进行适当认证相媲美。-->

查看者身份验证允许您限制私有应用程序的查看者。要访问您的应用程序，用户必须使用基于电子邮件的无密码登录或Google OAuth进行身份验证。要了解如何与查看者共享您的公共和私有应用程序的更多信息，请单击[此处](/streamlit-community-cloud/get-started/share-your-app)。

### 查看者是否需要访问GitHub存储库？

不需要！只有在您想要推送更改到应用程序时才需要访问GitHub存储库。

### 未经授权/已注销的查看者在查看我的应用程序时会看到什么？

为了避免向意外查看者提供任何关于您的应用程序的不必要信息，未经授权的查看者将看到404错误。在配置了查看者身份验证后，满足以下任何条件的用户在尝试查看您的应用程序时将看到404错误：

- 用户未使用Google SSO登录。
- 用户不在应用设置中提供的[观众列表](/streamlit-community-cloud/get-started/share-your-app#adding-viewers-from-the-app-dashboard)中。
- 用户没有对您的应用程序的GitHub存储库的读取访问权限。
<!-- - 用户对您的应用程序的GitHub存储库具有读取访问权限，但未注册Streamlit for Teams beta。 -->
- 用户对您的应用程序的GitHub存储库具有读取访问权限，但未注册Community Cloud。

![Four Oh Four](/images/streamlit-community-cloud/404.png)

### 我已经将某人添加到查看者列表中，但他们在尝试查看应用程序时仍然看到404错误

如果用户在将其电子邮件地址添加到查看者列表后仍然看到404错误，我们建议您：

- 检查用户是否通过单点登录登录了不同的Google帐户（如果您将其工作电子邮件地址添加到查看者列表中，请要求用户检查他们是否未登录到个人Google帐户，反之亦然）。
- 检查用户是否导航到了正确的URL。
- 检查用户在查看者列表中输入的电子邮件地址是否正确。
<!-- - 联系[support@streamlit.io](mailto:support@streamlit.io)，我们将很乐意帮助您。 -->
- 在我们的[社区论坛](https://discuss.streamlit.io/)上联系我们，我们将很乐意帮助您。

## 数据和应用程序安全性

### Streamlit如何保护我的数据？

Streamlit采取了多种行业最佳实践措施，以确保您的代码、数据和应用程序都是安全的。在我们的[信任和安全备忘录](/streamlit-community-cloud/trust-and-security)中阅读更多信息。

### 如何为我的组织设置SSO？

<!-- 如果您使用Google进行身份验证，那么您已经准备就绪。否则，请参考我们的[SSO配置指南](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)，了解如何与您选择的SSO提供商进行设置。 -->

Community Cloud默认使用Google OAuth进行身份验证。如果您使用Google进行身份验证，那么您已经准备就绪。

## 计费和管理

Community Cloud是一个免费服务。您不必担心设置计费或收费。

<!-- ### 是否有资源或用户数量的限制？

您的工作区的大小是根据用户、应用程序和这些应用程序的资源数量设定的。如果您接近限制，我们会通知您，并就是否要迁移到较大的工作区进行讨论。

### 我在哪里设置计费？

单击工作区右上角的“设置”按钮，打开“工作区设置”菜单。接下来，单击“计费和计划”选项卡，在Stripe中设置计费。

<div style={{ marginBottom: '-2em' }}>
  <Image src="/images/streamlit-community-cloud/setup-billing.png" />
</div>

### 何时开始收费？

入门计划永久免费，但如果您选择了团队帐户，则您的工作区将享有14天的免费试用期。

当您的试用即将结束时，您将收到自动发送的电子邮件提醒您设置账单信息（如果您还没有设置）以继续使用Teams计划。为了避免中断，我们建议您在试用结束前提前设置账单信息。一旦您设置了账单信息，我们将只在试用结束后开始向您收费。Stripe将每月自动向您的账户发送账单，并发送确认邮件给您。

如果您选择不设置计费方式，您的试用结束后将降级为入门计划。

## GitHub集成

### 为什么Streamlit需要额外的OAuth范围？

为了部署您的应用程序，Streamlit需要访问您的应用程序在GitHub上的源代码，并且还需要管理与存储库关联的公钥的能力。默认的GitHub OAuth范围足以处理公共GitHub存储库中的应用程序。但是，为了处理私有GitHub存储库中的应用程序，Streamlit需要从GitHub获取额外的`repo` OAuth范围。我们认识到，这个范围提供了Streamlit不需要的额外权限，而且作为一个重视安全性的人，我们宁愿不要获得这些权限。不幸的是，我们需要使用GitHub提供的API来完成工作。

### 在部署我的私有仓库应用程序后，我收到了GitHub发来的一封电子邮件，说有一个新的公钥被添加到了我的仓库中。这是正常情况吗？

**这是预期的行为**。当您尝试部署一个位于私有存储库中的应用程序时，Streamlit Community Cloud需要以某种方式访问该存储库。为此，我们创建了一个只读的[GitHub部署密钥](https://docs.github.com/en/free-pro-team@latest/developers/overview/managing-deploy-keys#deploy-keys)，然后使用公共SSH密钥访问您的存储库。当我们设置这个时，GitHub会通知存储库的管理员创建了该密钥作为安全措施。

### 当用户在GitHub上的权限发生变化时会发生什么？

一旦用户被添加到GitHub上的一个仓库中，他们最多需要等待15分钟才能在云上部署该应用程序。如果用户被从一个仓库中移除，他们的管理该应用程序的权限将在最多15分钟内被撤销。

## 限制和已知问题

以下是我们正在积极努力解决的一些限制和已知问题。如果您发现了问题，请[告诉我们](mailto:support@streamlit.io)！

- 当您将某些内容打印到云日志时，可能需要在其显示之前执行`sys.stdout.flush()`。
- 应用程序在运行Debian Buster（slim）和Python 3.7的Linux环境中执行。无法更改这些环境，并且我们可能在任何时间升级环境。如果我们升级环境，通常不会影响现有的应用程序，因此它们将继续按预期工作。但是，如果更新中存在关键修复，我们可能会强制升级所有应用程序。
- Matplotlib [与线程不兼容](https://matplotlib.org/3.3.2/faq/howto_faq.html#working-with-threads)。因此，如果您正在使用Matplotlib，应该像下面代码片段中所示，用锁包装您的代码。由于这个Matplotlib的bug在您共享应用程序时更容易出现，因为您更有可能获得更多的并发用户。

  ```python
  from matplotlib.backends.backend_agg import RendererAgg
  _lock = RendererAgg.lock

  with _lock:
    fig.title('这是一个图表')```
    ```python
fig.plot([1,20,3,40])
st.pyplot(fig)
```

- 所有的应用程序都托管在美国。目前无法进行配置。