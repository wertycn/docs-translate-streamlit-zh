---
slug: /streamlit-community-cloud/get-started/manage-your-app
title: Manage your app
---

# 管理您的应用程序

您可以直接从开发者视图中管理您的应用程序，也可以登录到您的应用程序仪表板[share.streamlit.io](https://share.streamlit.io/)，以查看、部署、删除、重启或收藏应用程序。

- [从开发者视图管理应用程序](#manage-apps-from-your-developer-view)
- [从应用程序仪表板管理应用程序](/streamlit-community-cloud/get-started/manage-your-app#manage-apps-from-your-app-dashboard)
- [在GitHub中管理应用程序](/streamlit-community-cloud/get-started/manage-your-app#manage-apps-in-github)
- [应用资源和限制](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)
- [应用程序收藏](/streamlit-community-cloud/get-started/manage-your-app#app-favoriting)
- [分析模态框](/streamlit-community-cloud/get-started/manage-your-app#analytics-modal)

## 从开发者视图管理应用程序

一旦您部署了一个应用程序，您将拥有该应用程序的开发者视图。

### 开发者视图

点击右下角的"管理应用"，以查看云日志和其他设置。

![开发者视图](/images/streamlit-community-cloud/developer-view.png)

### 云日志

一旦您点击"管理应用"，您将能够查看应用程序的日志。这是您排查应用程序问题的主要位置。

![云日志](/images/streamlit-community-cloud/cloud-logs.png)

您还可以点击 Cloud 日志底部的 "︙" 溢出菜单，查看应用的其他选项，包括下载日志、重启应用、删除应用、导航到设置（包括管理查看器访问权限和应用密钥）、进入应用仪表板、查看文档、联系支持或者退出登录。

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/cloud-logs-overflow.png" />
</div>

## 从应用程序仪表板管理应用程序

当您首次登录到share.streamlit.io时，您将进入应用程序仪表板，其中列出了您部署的所有应用程序。此列表包括您的工作区中其他开发人员部署的应用程序，因为您都是这些应用程序的管理员。这些应用程序用如下图标表示：

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/app-dashboard.png" />
</div>

### 应用工作空间

Streamlit Community Cloud 根据相应的 GitHub 仓库所有者自动将应用程序组织为工作空间。如果您是多个仓库的一部分，那么您将拥有多个工作空间。

![应用工作空间 1](/images/streamlit-community-cloud/app-workspaces-1.gif)

如果一个应用程序的 GitHub 仓库由您拥有，该应用程序将显示在您的个人工作空间中，名称为 "<YourGitHubHandle\>"。

如果一个应用的 GitHub 仓库是由一个组织（比如你的公司）拥有的，该应用将会出现在一个单独的工作区中，名为 "<GitHubOrganizationHandle\>"。

图片链接: ![App workspaces 2](/images/streamlit-community-cloud/app-workspaces-2.png)

图片链接: ![App workspaces 3](/images/streamlit-community-cloud/app-workspaces-3.jpg)

您还可以访问包含仅具有**查看权限**的应用程序的工作区。当您单击其相应的汉堡菜单时，这些应用程序将显示“仅查看”工具提示。

![App workspaces 4](/images/streamlit-community-cloud/app-workspaces-4.png)

要在工作区之间切换，请单击右上角列出的工作区，然后选择所需的工作区名称。

![App workspaces 5](/images/streamlit-community-cloud/app-workspaces-5.png)

### 重新启动应用程序

如果您的应用程序需要进行硬重启，请点击应用程序右侧的"︙"溢出菜单，然后点击"重启"。这将中断正在使用该应用程序的任何用户。您的应用程序重新部署可能需要几分钟的时间，在此期间，您和任何访问该应用程序的人都将看到"您的应用程序正在烤箱中"的屏幕。

![重启应用程序](/images/streamlit-community-cloud/reboot-an-app.png)

### 应用程序设置

应用程序设置允许您选择[自定义子域名](/streamlit-community-cloud/get-started/deploy-an-app#your-app-url)、[管理应用程序的查看者](/streamlit-community-cloud/get-started/share-your-app#adding-viewers-from-your-dashboard)和[应用程序的密钥](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)。点击链接了解更多关于这些功能的信息。

## 在 GitHub 中管理应用程序

### 更新您的应用程序

您的GitHub仓库是应用程序的源代码，这意味着每当您推送更新到仓库时，几乎可以实时地在应用程序中看到更新。试试看吧！

Streamlit还会智能检测您是否更改了依赖项，如果是的话，它将自动进行完整部署，这会花费一些时间。但由于大多数更新不涉及依赖项的更改，所以通常您应该可以实时看到应用程序的更新。

### 添加或删除依赖项

您可以通过更新 `requirements.txt`（Python依赖）或 `packages.txt`（Debian依赖）来随时添加/删除依赖项，并进行 `git push` 到您的远程仓库。这将导致Streamlit检测到其依赖项发生了变化，从而自动触发其安装。

最佳实践是在 `requirements.txt` 中固定您的 Streamlit 版本。否则，版本可能会在任何时候自动升级，而您可能不知情，这可能会导致意想不到的结果（例如，当我们在 Streamlit 中弃用某个功能时）。

## 应用程序资源和限制

### 资源限制

<!-- 具体的资源和限制将取决于[您的工作区计划](https://streamlit.io/cloud)。如果您需要更多的应用程序或更多的资源，您可以升级您的计划或联系[support@streamlit.io](mailto:support@streamlit.io)。 -->

所有社区云用户都可以访问相同的资源，并受到相同的限制（1 GB的RAM）。
如果您的应用程序运行缓慢或遇到了“Argh”页面，我们首先强烈建议您阅读以下博文并实施建议的内容，以防止应用程序超出资源限制，并检测您的Streamlit应用程序是否存在内存泄漏问题：

- [常见应用问题：资源限制](https://blog.streamlit.io/common-app-problems-resource-limits/)
- [修复应用程序内存泄漏的3个步骤](https://blog.streamlit.io/3-steps-to-fix-app-memory-leaks/)
<!-- 如果您需要更多的应用程序或更多的资源，您可以在我们的[社区论坛](https://discuss.streamlit.io/)上联系我们。 -->

#### 开发者视图

如果您的应用程序超出了资源限制，当您访问您的应用程序时，您将看到以下消息之一。如果您的应用程序使用的是旧版本的Streamlit（`<1.1.0`）且没有内存修复，您将在左侧看到该消息。如果您的应用程序使用的是新版本的Streamlit（`>=1.1.0`），您将在右侧看到该消息：

<Flex>
同样，您将会收到来自[alert@streamlit.io](mailto:alert@streamlit.io)的以下两种邮件之一，主题为"Your Streamlit app has gone over its resource limits 🤯":

<Flex>
<Image src="/images/streamlit-community-cloud/resource-limits-email-1.png" />
![图片](/images/streamlit-community-cloud/resource-limits-email-2.png)

非开发者视图

如果您的应用程序超过了资源限制，只有查看权限的用户在访问您的应用程序时将会看到以下消息之一。如果您的应用程序使用的是较旧版本的Streamlit（`<1.1.0`）且没有内存修复，则会显示左侧的消息；如果您的应用程序使用的是较新版本的Streamlit（`>=1.1.0`），则会显示右侧的消息。查看者可以选择在应用程序超过资源限制时通知您：

<Flex>
<Image src="/images/streamlit-community-cloud/resource-limits-viewer-1.png" />
<Image src="/images/streamlit-community-cloud/resource-limits-viewer-2.png" />
</Flex>

### 应用程序休眠

<!-- 在团队或企业计划中，私有应用程序不会休眠，但对于连续7天没有流量的公共和免费版应用程序，将自动进入休眠状态。这样做是为了减轻资源压力，允许平台的最佳共享使用！以下是关于此功能的一些需要了解的内容： -->

私有应用不会休眠，但公共社区云应用在连续7天没有流量的情况下将自动休眠。这样做是为了减轻资源负担，允许最佳的平台共享使用！以下是关于如何使用的一些需要了解的内容：

- 作为应用开发者，在您的应用上没有流量的5天后，您将收到一封电子邮件。
- 如果您想要保持应用的运行，您有两个选择之一：
  - 访问应用程序（创建流量）。
  - 将一个提交推送到应用程序（可以是空的）。
- 如果不进行任何操作，应用程序将在7天后进入休眠状态（在收到电子邮件后的2天）。当有人在此之后访问应用程序时，他们将看到休眠页面：
    <div style={{ maxWidth: '55%', marginBottom: '-3em', marginLeft: '5em' }}>
        <Image src="/images/spin_down.png" />
    </div>
- 要唤醒应用程序，请点击“是的，将此应用程序重新上线！”按钮。任何想查看应用程序的人都可以这样做，不仅仅是应用程序开发者！
- 您还可以通过Streamlit社区云仪表板唤醒应用程序。您将会知道哪些应用程序正在休眠，因为在应用程序设置旁边会出现一个月亮图标。要从仪表板唤醒应用程序，请点击月亮图标。
    <div style={{ maxWidth: '85%' }}>
        <Image src="/images/sleeping_app_moon.png" />
    </div>

## 应用程序收藏

Streamlit社区云支持"收藏"功能，可以让您从应用程序仪表板快速访问您的应用。收藏的应用将在应用仪表板的顶部显示一个黄色的星星（⭐）标记。您可以在任何您有权限访问的工作区中收藏和取消收藏应用。

<注意>

收藏是特定于您的账户的。您的工作区的其他成员无法看到您收藏了哪些应用。

</注意>

### 从应用仪表板中收藏一个应用程序

在应用仪表板上收藏应用程序有两种方法：

1. 将鼠标悬停在应用程序上，然后点击出现的星星（☆）。
   ![悬停收藏应用程序](/images/streamlit-community-cloud/favorite-app-dashboard-hover.png)

2. 点击应用程序右侧的“︙”溢出菜单，然后点击“收藏”。
   ![菜单收藏应用程序](/images/streamlit-community-cloud/favorite-app-dashboard-menu.png)

取消收藏应用程序的方法是：要么将鼠标悬停在应用程序上并再次点击星形标志（⭐），要么点击应用程序右侧的“︙”溢出菜单，然后点击“取消收藏”。

### 应用内收藏

您还可以直接在应用程序中收藏一个应用程序！目前，只有使用Streamlit v1.4.0或更高版本的应用程序才支持应用内收藏。请注意，对于您只具有查看权限的工作区中的应用程序，无法进行应用内收藏。

在您的工作区中查看任何应用程序时，点击应用程序右上角的星号（☆），位于“☰”汉堡菜单旁边。

![收藏应用程序](/images/streamlit-community-cloud/un-favorited.png)

要取消收藏应用程序，再次点击星号（⭐）。

![取消收藏应用程序](/images/streamlit-community-cloud/favorited-view.png)

<Tip>

点击[这里](/knowledge-base/deploy/upgrade-streamlit-version-on-streamlit-cloud)了解如何在Streamlit Community Cloud上升级应用程序的Streamlit版本。

</Tip>

## 分析模态框

一旦您拥有了Streamlit工作空间的访问权限，您将可以访问两种类型的分析数据：

1. [工作空间分析](/streamlit-community-cloud/get-started/manage-your-app#workspace-analytics)：显示有多少观众总共访问了您工作空间中的所有应用程序。
   <Image alt="Workspace analytics" src="/images/streamlit-community-cloud/workspace-analytics.gif" />
2. [App viewers](/streamlit-community-cloud/get-started/manage-your-app#app-viewers): shows you who has recently viewed your workspace’s individual apps and when.
   <Image alt="Workspace analytics" src="/images/streamlit-community-cloud/app-viewers-data.gif" />

<Note>

分析模态框对于具有访问工作区权限的所有人都可见，包括管理员、开发人员或具有工作区查看权限的任何人。

</Note>

### 工作区分析

Streamlit Community Cloud使您能够在一个中央仪表板中查看工作区中所有应用程序的分析数据。一目了然，您可以获得工作区活跃程度和应用程序受欢迎程度的概览。

要查看工作区分析：

1. 在仪表板标题上选择“**分析**”选项
   ![工作区分析仪表盘](/images/streamlit-community-cloud/workspace-analytics-header.png)
2. 在分析弹窗中查看"**工作区**"选项卡
   ![工作区分析弹窗](/images/streamlit-community-cloud/workspace-analytics-modal.png)

您将看到一个图表，可以将鼠标悬停在上面以查看当月查看了至少一个工作区中应用程序的用户数量。此查看次数包括工作区中任何人创建的应用程序。

实线表示仪表板上完全完成的月份，而虚线表示当前正在进行的月份。

<注意>

仪表板上的观众数据从2022年4月开始。2022年4月的数据是我们首次全面跟踪Streamlit工作区用户分析数据的月份，从2022年5月开始，我们的跟踪更加精细。

</注意>

### 应用程序观众

除了对工作区活动和应用程序的受欢迎程度进行总体概述外，Streamlit Community Cloud还允许您深入了解每个应用程序的详细情况并更好地了解其受众。

作为一个应用程序开发者或者一个有权限访问给定工作空间的查看者，您可以查看谁查看了给定应用程序以及何时查看的信息。具体来说，您可以查看您的应用程序的总查看者数量（从2022年4月份开始计算），最近的独立查看者（最多显示最近的20个查看者），以及他们上次查看的相对时间戳。

有三种方式可以访问应用程序的查看者数据：

1. 从应用程序仪表盘，点击应用程序右侧的“**︙**”菜单，选择**分析**：
   ![分析选项仪表盘](/images/streamlit-community-cloud/app-viewers-dashboard.png)

这样做会打开“**应用程序查看者**”选项卡中的“**分析**”模态窗口。
![应用程序查看者分析模态窗口](/images/streamlit-community-cloud/app-viewers-analytics-modal.png)

下拉菜单默认选择您的应用程序，并显示：

- 应用程序的总体（全部时间）独立查看者数量。
   - 一个按照最近观看者的姓名和相对时间戳（最新的在前）排序的最近观看者名单。

2. 在仪表板标题上点击“**Analytics**”选项，然后选择“**App viewers**”选项卡：
   ![Analytics option header](/images/streamlit-community-cloud/app-viewers-header.png)

   这样会打开“**Analytics**”模态框的“**App viewers**”选项卡。

   ![应用程序查看者分析下拉选项](/images/streamlit-community-cloud/app-viewers-dropdown-options.png)

默认情况下，工作区中的第一个应用程序在下拉菜单中被预先选中。您可以通过点击下拉菜单中对应的应用程序来选择您想要查看分析数据的应用程序。

![应用程序查看者分析下拉菜单](/images/streamlit-community-cloud/app-viewers-analytics-modal.png)

3. 您还可以直接从各个应用程序中访问应用程序查看器的分析数据！如果您对给定应用程序具有GitHub推送访问权限，则可以使用此功能。只需以开发者身份查看工作区中的任何应用程序，点击云日志底部的“**︙**”溢出菜单，然后选择“**分析**”：

<Flex>
<Image src="/images/streamlit-community-cloud/app-viewers-in-app.png" />
<Image src="/images/streamlit-community-cloud/app-viewers-analytics-modal.png" />
</Flex>

#### **公共应用程序的查看者与私有应用程序的查看者**

对于公共应用程序，我们会对您工作空间之外的所有查看者进行匿名处理，以保护他们的隐私，并将匿名查看者显示为随机化的假名。您仍然可以看到您工作空间中其他成员的身份。

与此同时，对于仅限您工作空间查看者访问的私有应用程序，您将能够看到最近查看您应用程序的具体用户。

此外，您可能偶尔会在私有应用程序中看到匿名用户。请放心，这些匿名用户确实具有您或您的工作区成员授予的授权查看访问权限。

用户以匿名身份出现的常见原因包括：

1. 应用程序先前是公开的
2. 查看者是在2022年4月观看应用程序时，Streamlit团队正在完善这个功能的用户识别
3. 查看者先前断开了他们的SSO和GitHub帐户

请查看Streamlit的通用[隐私声明](https://streamlit.io/privacy-policy)。