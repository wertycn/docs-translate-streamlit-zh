---
slug: /knowledge-base/deploy/resource-limits
title: Argh. This app has gone over its resource limits
---

# 哎呀，这个应用程序已经超过了资源限制

很抱歉！这意味着您已经达到了[Streamlit社区云](https://streamlit.io/cloud)账户的[资源限制](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)。

<!-- 一种避免这种情况的方法是[升级您的计划](https://streamlit.io/cloud)到具有更高资源限制的计划。  -->

您可以在应用程序中进行一些更改，以使其对资源的需求更少：

- 重启应用程序（临时解决方法）
- 使用`st.cache_data`或`st.cache_resource`只加载一次模型或数据
- 使用`ttl`或`max_entries`限制缓存的大小
- 将大型数据集移动到数据库中
- 对应用程序的内存使用进行性能分析

请查看我们的[博客文章](https://blog.streamlit.io/common-app-problems-resource-limits/)，了解有关如何防止您的应用程序超出Streamlit社区云的[资源限制](/streamlit-community-cloud/get-started/manage-your-app#app-resources-and-limits)的更深入的提示。

相关论坛帖子：

- <https://discuss.streamlit.io/t/common-app-problems-resource-limits/16969>
- <https://blog.streamlit.io/common-app-problems-resource-limits/>

我们仅针对非营利组织或教育机构按情况提供免费资源增加。如果您是非营利组织或教育机构，请填写[此表格](https://docs.google.com/forms/d/e/1FAIpQLSfzPNqrvH0HeaJnl0dtBgVV-ILqavzSmAEk84vDnMFIbvkGVA/viewform)，我们将尽快审核您的提交。

一旦增加完成，您将收到来自Streamlit营销团队的电子邮件，确认已经应用了增加操作。