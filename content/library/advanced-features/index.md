---
标题: 高级功能
链接: /library/advanced-features
---

## 高级功能

本部分将为您介绍Streamlit的不同部分是如何工作的。

<TileContainer>

<RefCard href="/library/advanced-features/cli" size="half">

##### 命令行选项

安装Streamlit时，还会安装一个命令行（CLI）工具。该工具的目的是运行Streamlit应用程序，更改Streamlit配置选项，并帮助您诊断和解决问题。

- [什么是命令行界面（CLI）？](/library/advanced-features/cli#command-line-interface)
- [如何从CLI中运行Streamlit应用程序？](/library/advanced-features/cli#run-streamlit-apps)
- [从CLI中查看Streamlit版本？](/library/advanced-features/cli#view-streamlit-version)
- [从CLI中查看文档](/library/advanced-features/cli#view-documentation)
- [从CLI中清除缓存](/library/advanced-features/cli#clear-cache)

</RefCard>

<RefCard href="/library/advanced-features/configuration" size="half">

##### Streamlit配置

Streamlit提供了四种不同的设置配置选项的方式。了解如何使用每种方式来改变Streamlit的行为。

- [如何设置配置选项？](/library/advanced-features/configuration)
- [选择不参与遥测数据收集](/library/advanced-features/configuration#telemetry)
- [查看所有配置选项](/library/advanced-features/configuration#view-all-configuration-options)

</RefCard>

<RefCard href="/library/advanced-features/theming" size="half">

##### 主题

本部分提供了示例，展示了Streamlit页面元素如何受各种主题配置选项的影响。

- [primaryColor](/library/advanced-features/theming#primarycolor)
- [backgroundColor](/library/advanced-features/theming#backgroundColor)
- [secondarybackgroundcolor](/library/advanced-features/theming#secondarybackgroundcolor)
- [textcolor](/library/advanced-features/theming#textcolor)
- [font](/library/advanced-features/theming#font)
- [base](/library/advanced-features/theming#base)

</RefCard>

<RefCard href="/library/advanced-features/caching" size="half">

##### 缓存

Streamlit缓存使得您的应用在从网络加载数据、操作大型数据集或进行耗时计算时保持高性能。要在Streamlit中缓存一个函数，您需要使用两个装饰器之一：`st.cache_data`和`st.cache_resource`。

- [最简示例](/library/advanced-features/caching#minimal-example)
- [基本用法](/library/advanced-features/caching#basic-usage)
  - [st.cache_data](/library/advanced-features/caching#stcache_data)
  - [st.cache_resource](/library/advanced-features/caching#stcache_resource)
  - [Deciding which caching decorator to use](/library/advanced-features/caching#deciding-which-caching-decorator-to-use)
- [Advanced usage](/library/advanced-features/caching#advanced-usage)
  - [Excluding input parameters](/library/advanced-features/caching#excluding-input-parameters)
  - [Controlling cache size and duration](/library/advanced-features/caching#controlling-cache-size-and-duration)
  - [自定义spinner](/library/advanced-features/caching#customizing-the-spinner)
  - [在缓存函数中使用Streamlit命令](/library/advanced-features/caching#using-streamlit-commands-in-cached-functions)
  - [变异和并发问题](/library/advanced-features/caching#mutation-and-concurrency-issues)
- [从st.cache迁移](/library/advanced-features/caching#migrating-from-stcache)

</RefCard>

<RefCard href="/library/advanced-features/session-state" size="half">

##### 为应用程序添加状态

会话状态是一种在每个用户会话中共享变量的方式。除了能够存储和持久化状态外，Streamlit还提供了使用回调函数来操作状态的能力。

- [什么是会话状态？](/library/advanced-features/session-state#what-is-state)
- [如何初始化会话状态项？](/library/advanced-features/session-state#initialization)
- [如何读取和更新会话状态项？](/library/advanced-features/session-state#reads-and-updates)
- [如何在会话状态中使用回调函数？](/library/advanced-features/session-state#example-2-session-state-and-callbacks)
- [如何在回调函数中使用`args`和`kwargs`？](/library/advanced-features/session-state#example-3-use-args-and-kwargs-in-callbacks)
- [如何在表单中使用回调函数？](/library/advanced-features/session-state#example-4-forms-and-callbacks)
- [会话状态与小部件状态有何关联?](/library/advanced-features/session-state#session-state-and-widget-state-association)
- [注意事项和限制](/library/advanced-features/session-state#caveats-and-limitations)

</RefCard>

<RefCard href="/library/advanced-features/prerelease" size="half">

##### 预发布功能

在Streamlit，我们喜欢快速推进并保持稳定性。为了更快地推进而不牺牲稳定性，我们为我们勇敢无畏的用户提供了两种尝试Streamlit最新功能的方式。

- [实验性功能](/library/advanced-features/prerelease#experimental-features)
- [每夜版发布](/library/advanced-features/prerelease#nightly-releases)

</RefCard>

<RefCard href="/library/advanced-features/secrets-management" size="half">

##### 密钥管理

本节提供了如何使用密钥管理在您的Streamlit应用中存储和检索敏感信息的示例。

- [本地开发和设置密钥](/library/advanced-features/secrets-management#develop-locally-and-set-up-secrets)
- [在您的应用中使用密钥](/library/advanced-features/secrets-management#use-secrets-in-your-app)
- [错误处理](/library/advanced-features/secrets-management#error-handling)
- [在Streamlit Community Cloud上使用密钥](/library/advanced-features/secrets-management#use-secrets-on-streamlit-community-cloud)

</RefCard>

<RefCard href="/library/advanced-features/timezone-handling" size="half">

##### 使用时区

处理时区可能会很棘手。本节提供了如何在Streamlit中处理时区以避免意外行为的高级说明。

- [概述](/library/advanced-features/timezone-handling#working-with-timezones)
- [Streamlit如何处理时区](/library/advanced-features/timezone-handling#how-streamlit-handles-timezones)
- [没有时区的`datetime`实例（无时区）](/library/advanced-features/timezone-handling#datetime-instance-without-a-timezone-naive)
- [带有时区的`datetime`实例](/library/advanced-features/timezone-handling#datetime-instance-with-a-timezone)

</RefCard>

<RefCard href="/library/advanced-features/widget-semantics" size="full">

##### 关于小部件行为的高级说明

小部件是神奇的，通常会按照您的期望工作。但在某些情况下，它们可能会有一些令人惊讶的行为。本节提供了关于小部件行为的高级、抽象描述，包括一些常见的特殊情况。

</RefCard>
</TileContainer>
