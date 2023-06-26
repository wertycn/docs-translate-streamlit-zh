---
title: 故障排除
slug: /streamlit-community-cloud/troubleshooting
---

# 故障排除

很抱歉听到您遇到了问题！请查看下面的一些常见问题和解决方案。如果您找不到解决办法，请在我们的[社区论坛](http://discuss.streamlit.io)上发布问题，以便我们的工程师或社区成员帮助您。

## 目录

1. [常见帮助](#general-help)
2. [应用程序部署](#deploying-apps)
3. [分享和访问应用程序](/streamlit-community-cloud/troubleshooting#sharing-and-accessing-apps)
4. [数据和应用程序安全性](/streamlit-community-cloud/troubleshooting#data-and-app-security)
<!-- 5. [计费和管理](/streamlit-community-cloud/troubleshooting#billing-and-administration) -->
5. [GitHub集成](/streamlit-community-cloud/troubleshooting#github-integration)
6. [限制和已知问题](/streamlit-community-cloud/troubleshooting#限制和已知问题)

## 一般帮助

### 如何获取关于我的应用程序的帮助？

如果您有任何问题、反馈、遇到任何问题或需要联系我们，您可以在我们的[社区论坛](https://discuss.streamlit.io/)上提问。这对于与开源库和社区云相关的任何问题都是最合适的 - 调试代码、部署、资源限制等。

<!-- - 在[论坛](https://discuss.streamlit.io)上提问⬅️ 这是关于开源库的任何问题的最佳方式
- 发送电子邮件至[support@streamlit.io](mailto:support@streamlit.io)⬅️ 这是与应用平台相关的任何事情 - 部署、SSO等的最佳方式

如果您愿意，我们也很乐意帮助您开发应用程序。如果您想分享一些代码或让我们的团队查看您的应用程序并给您一些建议，请给我们发个消息。 -->

## 部署应用程序

### 我的仓库在部署页面上没有显示出来

有可能即使已经存在，它也不会显示出来。请尝试手动输入。如果我们无法识别它，您将会看到下面的消息，并提供一个链接以便点击并获取访问权限。

![部署页面故障排除](/images/streamlit-community-cloud/troubleshooting-deploy-page.png)

如果因某种原因无法正常工作，请尝试退出并重新登录，以确保更改生效。如果还是无法解决，请告诉我们，我们会帮助您解决问题！

### 我无法部署应用程序

部署应用程序时，您必须具有对GitHub存储库的管理员级访问权限。请与您的管理员确认您是否具有此权限。如果没有，请请求他们首次部署（这样我们才能建立用于持续集成的Webhook），然后您可以推送应用程序的更新。

### 我需要为我的应用程序设置特定的Python版本

在部署应用程序时，在高级设置中，您可以选择希望应用程序使用的Python版本。

![Streamlit Community Cloud高级设置](/images/streamlit-community-cloud/advanced-settings.png)

### 如何将文件存储在本地？

如果您希望将数据存储在本地而不是数据库中，您可以将文件存储在GitHub存储库中。Streamlit只是Python，所以您可以使用以下方式读取文件：

`pandas.read_csv("data.csv")`或`open("data.csv")`

<Tip>

如果您有非常大的或二进制数据，并且频繁更改，而且git感觉很慢，您可能希望查看[Git Large File Store (LFS)](https://git-lfs.github.com/)，这是一种更好的在GitHub中存储大文件的方法。您无需对应用程序进行任何更改即可开始使用它。如果您的GitHub仓库使用LFS，它将与Streamlit一起“正常工作”。

</提示>

### 在部署过程中，我的应用程序遇到了问题

请点击屏幕右下角的"管理应用程序"展开按钮，查看云日志。通常问题是由于未声明的依赖项引起的。有关[依赖项管理的更多信息](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies)，请参阅此处。

<!-- 如果这不是问题的原因，请将您看到的日志和警告发送给您的Streamlit联系人，我们将帮助您解决！ -->

如果这不是问题的原因，请将您看到的日志和警告发送到我们的[社区论坛](https://discuss.streamlit.io/)，我们将帮助您解决！
### 我的应用程序遇到资源限制/应用程序运行非常缓慢

如果您的应用程序运行缓慢或遇到了'Argh'页面，我们首先强烈推荐您阅读以下博文的建议，以防止应用程序达到资源限制并检测Streamlit应用程序是否存在内存泄漏：

- [常见应用程序问题：资源限制](https://blog.streamlit.io/common-app-problems-resource-limits/)
- [修复应用程序内存泄漏的3个步骤](https://blog.streamlit.io/3-steps-to-fix-app-memory-leaks/)

如果您仍然遇到问题，请单击[此处](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)了解有关资源限制的更多信息。

### 我可以为我的应用程序获得自定义URL吗？

是的！您可以在[此处找到设置自定义子域的说明](/knowledge-base/deploy/custom-subdomains)。

## 分享和访问应用程序

### 我没有SSO。如何登录Streamlit？

没有单点登录(SSO)？没问题！您可以使用您的电子邮件地址登录Streamlit。点击[这里](/streamlit-community-cloud/get-started#sign-in-with-email)获取逐步指南，了解如何使用电子邮件登录。

<!-- ### 如何将开发人员添加到我的Streamlit for Teams帐户？

如果您在同一个GitHub存储库上，那么您将自动添加到同一个工作区。只需邀请他们登录[share.streamlit.io](http://share.streamlit.io)，一旦他们连接了GitHub帐户，我们将自动将他们路由到您的工作区。

### 如何向我的Streamlit应用程序添加观众？

<!-- 默认情况下，使用Streamlit Community Cloud Teams和Enterprise部署的所有应用程序都是私有的，这意味着除非您明确授权，否则其他公司内的人无法查看它们。要添加查看者，请使用您的组织的SSO提供商[配置单点登录](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)。

如果您无法实现单点登录，但希望使用密码来保护您的Streamlit应用程序，请阅读我们的[无SSO身份验证](/knowledge-base/deploy/authentication-without-sso)指南。注意：虽然这种技术增加了一定的安全性，但它**不能**与使用SSO提供者进行适当的身份验证相媲美。

查看器权限允许您限制私有应用的查看者。要访问您的应用程序，用户必须使用基于电子邮件的无密码登录或Google OAuth进行身份验证。要了解有关如何与查看者共享公共和私有应用程序的更多信息，请点击[这里](/streamlit-community-cloud/get-started/share-your-app)。

### 查看者是否需要访问GitHub存储库？

不需要！只有在您想要将更改推送到应用程序时，您才需要访问GitHub存储库。

### 未经授权/已注销的观众在查看我的应用程序时会看到什么？

未经授权的观众将显示一个404错误，以避免向意外的观众提供关于您的应用程序的任何不必要的信息。在配置了观众身份验证之后，满足以下任何条件的用户在尝试查看您的应用程序时将看到404错误：

- 用户未使用Google SSO登录。
- 用户未包含在应用设置中提供的[查看者列表](/streamlit-community-cloud/get-started/share-your-app#adding-viewers-from-the-app-dashboard)中。
- 用户缺乏对您的应用程序 GitHub 存储库的读取权限。
<!-- - 用户对您的应用程序 GitHub 存储库具有读取权限，但未加入 Streamlit for Teams beta。 -->
- 用户对您的应用程序 GitHub 存储库具有读取权限，但未加入 Community Cloud。

![Four Oh Four](/images/streamlit-community-cloud/404.png)

### 我已经将某人添加到查看者列表中，但他们在尝试查看应用程序时仍然看到404错误

如果用户在将其电子邮件地址添加到查看者列表后仍然看到404错误，我们建议您执行以下操作：

- 检查用户是否通过单一登录（Single Sign-On）登录了不同的Google帐号（如果您已将他们的工作电子邮件地址添加到查看者列表，请要求用户检查他们是否登录了他们个人的Google帐号，反之亦然）。
- 检查用户是否导航到了正确的URL。
- 检查用户的电子邮件地址是否在查看器列表中正确输入。
<!-- - 联系[support@streamlit.io](mailto:support@streamlit.io)，我们将很乐意提供帮助。 -->
- 在我们的[社区论坛](https://discuss.streamlit.io/)上联系我们，我们将很乐意提供帮助。

## 数据和应用程序安全

### Streamlit如何保护我的数据？

Streamlit采取了许多行业最佳实践措施，以确保您的代码、数据和应用程序都是安全的。请在我们的[信任和安全备忘录](/streamlit-community-cloud/trust-and-security)中阅读更多信息。

### 如何为我的组织设置SSO？

<!-- 如果您使用Google进行身份验证，那么您已经准备就绪。否则，请参考我们的[SSO配置指南](/streamlit-community-cloud/get-started/share-your-app/configuring-single-on-sso)，了解如何与您选择的SSO提供商进行设置。-->

Community Cloud默认使用Google OAuth进行身份验证。如果您使用Google进行身份验证，您已经准备就绪。

## 计费和管理

Community Cloud是一个免费服务。您不必担心设置计费或被收取费用。

<!-- ### 我的资源或用户数量有限制吗？

您的工作区的用户数量、应用程序数量以及这些应用程序的资源都有限制。如果您接近限制，我们会通知您，并与您讨论是否需要迁移到更大的工作区。

### 如何设置计费？

点击工作区右上角的“设置”按钮，打开“工作区设置”菜单。然后，点击“计费和计划”选项卡，在Stripe中设置计费。

<div style={{ marginBottom: '-2em' }}>
  <Image src="/images/streamlit-community-cloud/setup-billing.png" />
</div>

### 何时收费？

Starter计划永久免费，但如果您选择了Teams账户，则您的工作区将有14天的免费试用期。

当您的试用期接近结束时，您将收到自动发送的电子邮件提醒您设置您的计费信息（如果您还没有设置）以继续使用您的团队计划。为了避免中断，我们建议您在试用期结束前提前设置计费信息。一旦您设置了计费信息，我们将只在试用期结束后开始向您收费。Stripe将每月自动向您的账户进行结算，并发送确认电子邮件给您。

如果您选择不设置计费方式，您的试用期结束后将会降级为入门版计划。-->

## GitHub集成

### Streamlit为什么需要额外的OAuth权限范围？

为了部署您的应用程序，Streamlit需要访问您的应用程序在GitHub上的源代码，并且能够管理与存储库关联的公共密钥。默认的GitHub OAuth范围足以处理公共GitHub存储库中的应用程序。然而，为了处理私有GitHub存储库中的应用程序，Streamlit需要从GitHub获取额外的`repo` OAuth范围。我们意识到这个范围提供了Streamlit不需要的额外权限，作为重视安全性的人，我们宁愿不被授予这些权限。不幸的是，我们需要使用GitHub提供的API来工作。

### 在部署我的私有仓库应用程序后，我收到了 GitHub 发来的一封邮件，说我的仓库中添加了一个新的公钥。这是正常的吗？

**这是预期的行为**。当您尝试部署一个位于私有存储库中的应用程序时，Streamlit社区云需要以某种方式访问该存储库。为此，我们创建了一个只读的[GitHub部署密钥](https://docs.github.com/en/free-pro-team@latest/developers/overview/managing-deploy-keys#deploy-keys)，然后使用公共SSH密钥访问您的存储库。当我们设置这个密钥时，GitHub会通知存储库的管理员，作为一种安全措施。

### 当用户在GitHub上的权限发生变化时会发生什么？

一旦用户被添加到GitHub上的一个仓库中，他们最多需要等待15分钟才能在云端部署应用程序。如果用户被从GitHub上的一个仓库中移除，他们将在最多15分钟内失去在该仓库中管理应用程序的权限。

## 限制和已知问题

以下是一些我们正在积极努力解决的限制和已知问题。如果您发现了问题，请[告诉我们](mailto:support@streamlit.io)！

- 当您将某些内容打印到云日志时，可能需要在其显示之前执行`sys.stdout.flush()`。
- 应用程序在运行Debian Buster (slim)的Linux环境中执行，使用Python 3.7。无法更改这些配置，并且我们可以在任何时候升级环境。如果我们升级了环境，通常不会影响现有的应用程序，它们将如预期一样继续工作。但是，如果更新中有关键修复，我们可能会强制升级所有应用程序。
- Matplotlib [无法很好地与线程一起使用](https://matplotlib.org/3.3.2/faq/howto_faq.html#working-with-threads)。因此，如果您在使用Matplotlib，您应该像下面的代码片段中所示，用锁包装您的代码。这个Matplotlib的bug在共享应用程序时更加突出，因为您很可能会获得更多的并发用户。

  ```python
  from matplotlib.backends.backend_agg import RendererAgg
  _lock = RendererAgg.lock

  with _lock:
    fig.title('这是一个图像')```
    ```
fig.plot([1,20,3,40])
st.pyplot(fig)
```

- 所有应用程序都托管在美国。目前不支持自定义配置。
