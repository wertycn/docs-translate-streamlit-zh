---
title: 管理您的应用程序
slug: /streamlit-community-cloud/get-started/manage-your-app
---

# 管理您的应用程序

您可以直接从您的开发者视图中管理您的应用程序，也可以登录到您的应用程序仪表板在 [share.streamlit.io](https://share.streamlit.io/) 上查看、部署、删除、重启或收藏应用程序。

- [从您的开发者视图管理应用程序](#manage-apps-from-your-developer-view)
- [从应用仪表板管理应用](/streamlit-community-cloud/get-started/manage-your-app#manage-apps-from-your-app-dashboard)
- [在GitHub中管理应用](/streamlit-community-cloud/get-started/manage-your-app#manage-apps-in-github)
- [应用资源和限制](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)
- [应用收藏](/streamlit-community-cloud/get-started/manage-your-app#app-favoriting)
- [分析模态框](/streamlit-community-cloud/get-started/manage-your-app#analytics-modal)

## 从开发者视图管理应用程序

一旦您部署了一个应用程序，您将拥有该应用程序的开发者视图。

### 开发者视图

点击右下角的"管理应用程序"，以查看您的云日志和其他设置。

![开发者视图](/images/streamlit-community-cloud/developer-view.png)

### 云日志

点击“管理应用程序”后，您将能够查看您的应用程序的日志。这是您解决应用程序任何问题的主要位置。

![Cloud logs](/images/streamlit-community-cloud/cloud-logs.png)

您还可以点击云日志底部的"︙"溢出菜单，查看应用程序的其他选项，包括下载日志、重启应用程序、删除应用程序、导航到设置（包括管理查看者访问和应用程序密钥）、转到应用程序仪表板、转到文档、联系支持或注销。

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/cloud-logs-overflow.png" />
</div>

## 从应用程序仪表板管理应用程序

当您首次登录到share.streamlit.io时，您将进入应用程序仪表板，该仪表板显示您部署的所有应用程序的列表。此列表包括您工作区中其他开发人员部署的应用程序，因为您都是这些应用程序的管理员。这些应用程序的图标如下所示：

<div style={{ maxWidth: '45%', marginBottom: '-3em', marginLeft: '10em' }}>
    <Image src="/images/streamlit-community-cloud/app-dashboard.png" />
</div>

### 应用工作区

Streamlit社区云按照相应的GitHub代码仓库的所有者自动将应用程序分组到工作区中。如果您参与多个代码仓库，则会有多个工作区。

![应用工作区 1](/images/streamlit-community-cloud/app-workspaces-1.gif)

如果一个应用的GitHub代码仓库是您拥有的，该应用将出现在您的个人工作区中，名称为"<您的GitHub用户名\>"。

![应用程序工作区2](/images/streamlit-community-cloud/app-workspaces-2.png)

如果一个应用程序的GitHub仓库是由**一个组织**（比如您的公司）拥有的，该应用程序将出现在一个单独的工作区中，名为"<GitHubOrganizationHandle\>"。

![应用程序工作区3](/images/streamlit-community-cloud/app-workspaces-3.jpg)

您还可以访问包含您仅具有**查看权限**的应用程序的工作区。当您点击它们各自的汉堡菜单时，这些应用程序将显示“仅查看”工具提示。

![App workspaces 4](/images/streamlit-community-cloud/app-workspaces-4.png)

要在工作区之间切换，请点击右上角列出的工作区，然后选择所需的工作区名称。

![App workspaces 5](/images/streamlit-community-cloud/app-workspaces-5.png)

### 重新启动应用程序

如果您的应用程序需要进行硬重启，请点击应用程序右侧的"︙"溢出菜单，然后点击重新启动。这将中断当前正在使用该应用程序的任何用户。此外，在应用程序重新部署的几分钟内，您和任何访问应用程序的人将看到"Your app is in the oven"的屏幕。

![重新启动应用程序](/images/streamlit-community-cloud/reboot-an-app.png)

### 应用程序设置

应用程序设置允许您选择一个自定义子域名作为您的应用程序的URL，管理您应用程序的观看者以及应用程序的秘密。点击链接了解更多关于这些功能的信息。

## 在GitHub中管理应用程序

### 更新您的应用程序

您的GitHub存储库是该应用程序的源代码，这意味着每当您推送更新到存储库时，您将几乎实时地在应用程序中看到更新。尝试一下吧！

Streamlit还会智能检测您是否更改了依赖项，如果是这样，它将自动进行完整的重新部署，这需要一些时间。但由于大多数更新不涉及依赖项的更改，您通常会实时看到应用程序的更新。

### 添加或删除依赖项

您可以通过更新`requirements.txt`（Python依赖）或`packages.txt`（Debian依赖）来在任何时候添加/删除依赖项，并执行`git push`到您的远程仓库。这将导致Streamlit检测到其依赖项发生了更改，并自动触发其安装。

最佳实践是将Streamlit版本固定在`requirements.txt`中。否则，版本可能在任何时候自动升级，而您可能并不知情，这可能会导致不希望的结果（例如，在Streamlit中弃用某个功能时）。

## 应用程序资源和限制

### 资源限制

<!-- 具体的资源和限制将取决于 [您的工作区计划](https://streamlit.io/cloud)。如果您需要更多应用程序或更多资源，您可以升级您的计划或联系 [support@streamlit.io](mailto:support@streamlit.io)。-->

所有社区云用户都可以访问相同的资源，并受到相同的限制（1 GB RAM）。
如果您的应用程序运行缓慢或遇到了"Argh"页面，我们强烈建议您首先查看并实施以下博客文章中的建议，以防止应用程序超过资源限制并检测Streamlit应用程序是否存在内存泄漏：

- [常见应用程序问题：资源限制](https://blog.streamlit.io/common-app-problems-resource-limits/)
- [修复应用程序内存泄漏的3个步骤](https://blog.streamlit.io/3-steps-to-fix-app-memory-leaks/)
<!-- 如果您需要更多的应用程序或更多的资源，您可以在我们的[社区论坛](https://discuss.streamlit.io/)上联系我们。 -->

#### 开发者视图

如果您的应用程序超过了资源限制，当您访问您的应用程序时，您将看到以下消息之一。如果您的应用程序使用的是较旧的Streamlit版本（`<1.1.0`）且没有内存修复，您将看到左侧的消息。如果您的应用程序使用的是较新的Streamlit版本（`>=1.1.0`），您将看到右侧的消息：

<Flex>
类似地，您将从[alert@streamlit.io](mailto:alert@streamlit.io)收到以下两封邮件之一，主题为"您的Streamlit应用程序已超过资源限制 🤯"：

<Flex>
<Image src="/images/streamlit-community-cloud/resource-limits-email-1.png" />
<Image src="/images/streamlit-community-cloud/resource-limits-email-2.png" />
</Flex>

#### Non-developer view

如果您的应用程序超出了资源限制，只有查看权限的用户在访问应用程序时会看到以下消息之一。如果您的应用程序使用旧版本的Streamlit（`<1.1.0`）且没有内存修复程序，则会显示左侧的消息；如果您的应用程序使用较新版本的Streamlit（`>=1.1.0`），则会显示右侧的消息。查看者可以选择在应用程序超出资源限制时通知您：

<Flex>
<Image src="/images/streamlit-community-cloud/resource-limits-viewer-1.png" />
<Image src="/images/streamlit-community-cloud/resource-limits-viewer-2.png" />
</Flex>

### 应用程序休眠

<!-- 在 Teams 或企业计划上的私有应用不会休眠，但对于连续7天没有流量的公共和免费版应用，将自动进入休眠状态。这是为了缓解资源压力并允许平台的最佳共享使用！以下是关于此功能的一些需要了解的内容： -->

私有应用不会休眠，但是连续7天没有流量的公共社区云应用程序将自动进入休眠状态。这样做是为了减轻资源负担，并允许平台的最佳共享使用！以下是关于此功能的一些需要了解的内容：

- 作为应用程序开发者，您将在应用程序没有流量的5天后收到一封电子邮件。
- 如果您希望保持应用程序的运行状态，您有两个选择之一：
  - 访问应用程序（产生流量）。
  - 向应用程序提交一个提交（可以是空的）。
- 如果未操作，应用程序将在7天后进入休眠状态（在您收到电子邮件后的2天）。当有人在此之后访问应用程序时，他们将看到休眠页面：
    <div style={{ maxWidth: '55%', marginBottom: '-3em', marginLeft: '5em' }}>
        <Image src="/images/spin_down.png" />
    </div>
- 要唤醒应用程序，请按下“是的，将此应用程序重新启动！”按钮。这可以由*任何人*来查看应用程序，而不仅仅是应用程序开发人员！
- 您也可以通过Streamlit Community Cloud仪表板唤醒应用程序。您将知道哪些应用程序处于休眠状态，因为应用程序设置旁边将显示一个月亮图标。要从仪表板唤醒应用程序，请单击月亮。
    <div style={{ maxWidth: '85%' }}>
        <Image src="/images/sleeping_app_moon.png" />
    </div>

## 应用程序收藏

Streamlit社区云支持"收藏"功能，让您可以从应用程序仪表板快速访问您的应用程序。收藏的应用程序会在应用程序仪表板的顶部显示一个黄色的星星（⭐）。您可以在任何您有权限的工作区中收藏和取消收藏应用程序。

<注意>

收藏只针对您的账户有效。您的工作区其他成员无法看到您收藏的应用程序。

</注意>

### 从应用程序仪表板收藏一个应用程序

有两种方法可以从应用程序仪表板中收藏一个应用程序:

1. 将鼠标悬停在一个应用程序上，然后点击出现的星星 (☆)。
   ![悬停收藏应用程序](/images/streamlit-community-cloud/favorite-app-dashboard-hover.png)
2. 点击应用程序右侧的"︙"溢出菜单，然后点击收藏。
   ![菜单收藏应用程序](/images/streamlit-community-cloud/favorite-app-dashboard-menu.png)

取消收藏应用程序的方法是，将鼠标悬停在应用程序上并再次点击星星(⭐)，或者点击应用程序右侧的"︙"溢出菜单，然后点击"取消收藏"。

### 应用内收藏

您还可以在应用程序内部收藏应用程序！目前，应用内收藏仅适用于使用Streamlit v1.4.0或更高版本的应用程序。请注意，在您只能查看工作区中的应用程序上，无法使用应用内收藏功能。

在您的工作区中查看任何应用程序时，点击应用程序右上角的星星（☆）按钮，位于“☰”汉堡菜单旁边。

![应用程序收藏](/images/streamlit-community-cloud/un-favorited.png)

要取消收藏一个应用程序，再次点击星星（⭐）按钮。

![取消应用程序收藏](/images/streamlit-community-cloud/favorited-view.png)

<Tip>（提示）

点击[这里](/knowledge-base/deploy/upgrade-streamlit-version-on-streamlit-cloud)了解如何在Streamlit Community Cloud上升级您的应用程序的Streamlit版本。

</Tip>

## 分析模态框

一旦您获得了Streamlit工作区的访问权限，您就可以访问两种类型的分析数据:

1. [工作区分析](/streamlit-community-cloud/get-started/manage-your-app#workspace-analytics): 显示了访问了工作区中所有应用程序的观众总数。
   <Image alt="工作区分析" src="/images/streamlit-community-cloud/workspace-analytics.gif" />
2. [应用程序查看者](/streamlit-community-cloud/get-started/manage-your-app#app-viewers): 显示最近查看工作区中各个应用程序的人员和时间。
   <Image alt="应用程序查看者数据" src="/images/streamlit-community-cloud/app-viewers-data.gif" />

<Note>

分析模态框对您的工作区中所有具有访问权限的人可见，包括管理员、开发者或具有工作区查看权限的任何人。

</Note>

### 工作区分析

Streamlit社区云使您能够在一个集中的仪表板中查看工作区中所有应用程序的分析数据。一目了然，您可以了解工作区的活跃程度和应用程序的受欢迎程度。

要查看工作区分析，请执行以下操作：

1. 在仪表板标题上选择“**分析**”选项
   ![工作区分析仪表盘](/images/streamlit-community-cloud/workspace-analytics-header.png)
2. 在分析模态框中查看 "**工作区**" 选项卡
   ![工作区分析模态框](/images/streamlit-community-cloud/workspace-analytics-modal.png)

您将看到一个图表，您可以将鼠标悬停在上面，以查看当月至少查看了一个工作区应用的用户数量。此查看计数包括您工作区中的任何人创建的应用程序。

实线表示仪表板上完全完成的月份，而虚线表示当前进行中的月份。

<Note>

您的仪表板上的观众数据从2022年4月开始。2022年4月是我们全面跟踪Streamlit工作区用户分析的第一个月，从2022年5月开始，我们的跟踪更加精细。

</Note>

### 应用程序观众数

除了对工作区活动和应用程序受欢迎程度的总体概述之外，Streamlit Community Cloud还允许您深入到单个应用程序的级别，更好地了解其用户浏览情况。

作为应用程序开发者或具有访问给定工作区的查看者，您可以查看谁查看了给定的应用程序以及何时查看。具体而言，您可以查看您的应用程序的总查看数（从2022年4月开始），最近的独立查看者（最多显示最近的20个查看者），以及他们最后一次查看的相对时间戳。

有三种方式可以访问应用程序查看者的数据：

1. 从应用程序仪表板中，点击应用程序右侧的 "**︙**" 溢出菜单，然后选择 **分析**：
   ![分析选项仪表板](/images/streamlit-community-cloud/app-viewers-dashboard.png)

   这样做会打开“**应用程序查看者**”选项卡的“**分析**”模态框。
   ![应用程序查看者分析模态框](/images/streamlit-community-cloud/app-viewers-analytics-modal.png)

   默认情况下，下拉菜单会选择您的应用程序并显示：

   - 应用程序的总体（历史）唯一查看者数量。
   - 一个按照最近观看者的姓名和相对时间戳进行排序的观看者名单，时间戳表示他们上次观看的时间（最新的在前）。

2. 在仪表盘标题中点击 "**Analytics**" 选项，然后选择 "**App viewers**" 标签页：
   ![Analytics option header](/images/streamlit-community-cloud/app-viewers-header.png)

   这样会打开 "**Analytics**" 弹出窗口的 "**App viewers**" 标签页。

   ![应用程序查看者分析下拉选项](/images/streamlit-community-cloud/app-viewers-dropdown-options.png)

默认情况下，工作区中的第一个应用程序在下拉菜单中预先选择。您可以通过点击下拉菜单中对应的应用程序来选择您想查看分析数据的应用程序。

![应用程序查看者分析下拉菜单](/images/streamlit-community-cloud/app-viewers-analytics-modal.png)

3. 您还可以直接从各个应用程序中访问应用程序查看器的分析数据！如果您对给定应用程序具有GitHub推送访问权限，则可以使用此功能。只需以开发者身份查看工作区中的任何应用程序，点击云日志底部的 "**︙**" 溢出菜单，然后选择 "**Analytics**"：

<Flex>
<Image src="/images/streamlit-community-cloud/app-viewers-in-app.png" />
<Image src="/images/streamlit-community-cloud/app-viewers-analytics-modal.png" />
</Flex>

#### **公共应用程序的查看者与私有应用程序的查看者**

对于公共应用程序，我们会对工作区外的所有查看者进行匿名处理，以保护他们的隐私，并将匿名查看者显示为随机的化名。但您仍然可以看到工作区中其他成员的身份。

而对于只能由您自己工作区的查看者访问的私有应用程序，您将能够看到最近查看您应用程序的具体用户。

此外，您有时可能会在私有应用中看到匿名用户。请放心，这些匿名用户确实具有您或您的工作区成员授予的授权查看权限。

用户以匿名身份出现的常见原因包括：

1. 应用程序以前是公开的
2. 在2022年4月，Streamlit团队对此功能进行用户识别时，给予了查看者查看应用程序的权限
3. 查看者之前断开了他们的SSO和GitHub账户

请查看Streamlit的通用[隐私政策](https://streamlit.io/privacy-policy)。
