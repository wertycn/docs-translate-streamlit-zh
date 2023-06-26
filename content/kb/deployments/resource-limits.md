---
title: 哎呀，这个应用程序已经超过了资源限制
slug: /knowledge-base/deploy/resource-limits
---

# 哎呀，这个应用程序已经超过了资源限制

抱歉！这意味着您已经达到了[Streamlit社区云](https://streamlit.io/cloud)帐户的[资源限制](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)。

<!-- 一个避免超过资源限制的方法是将您的计划升级到具有更高资源限制的计划。 -->

有几个方法可以使您的应用程序更加节省资源：

- 重新启动应用程序（临时解决方法）
- 使用`st.cache_data`或`st.cache_resource`仅加载一次模型或数据
- 使用`ttl`或`max_entries`限制缓存大小
- 将大型数据集移动到数据库中
- 分析应用程序的内存使用情况

请查看我们的[博客文章](https://blog.streamlit.io/common-app-problems-resource-limits/)，了解有关“常见应用问题：资源限制”的更详细提示，以避免您的应用程序达到Streamlit社区云的[资源限制](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)。

相关论坛帖子：

- <https://discuss.streamlit.io/t/common-app-problems-resource-limits/16969>
- [https://blog.streamlit.io/common-app-problems-resource-limits/](https://blog.streamlit.io/common-app-problems-resource-limits/)

我们只针对非营利组织或教育机构根据具体情况提供免费的资源增加。如果您是非营利组织或教育机构，请填写[此表格](https://docs.google.com/forms/d/e/1FAIpQLSfzPNqrvH0HeaJnl0dtBgVV-ILqavzSmAEk84vDnMFIbvkGVA/viewform)，我们会尽快审核您的申请。

一旦增加完成，您将收到来自Streamlit营销团队的电子邮件，确认增加已生效。
