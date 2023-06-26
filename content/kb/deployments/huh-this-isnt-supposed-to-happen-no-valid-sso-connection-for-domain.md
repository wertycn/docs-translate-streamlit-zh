---
title: 哎呀，这不应该发生的。域名没有有效的SSO连接
slug: /knowledge-base/deploy/huh-this-isnt-supposed-to-happen-no-valid-sso-connection-for-domain
---

# 哎呀，这不应该发生的。域名没有有效的SSO连接

本知识库文章将帮助您解决在使用SSO登录Streamlit社区云时出现的“域名没有有效的SSO连接”错误。

## 问题

当您尝试使用SSO（SAML身份验证）登录Streamlit社区云账户时，您会看到以下屏幕：

![没有有效的SSO连接](/images/knowledge-base/no-valid-sso-connection-for-this-domain.png)

这个消息表示您过去曾使用GitHub和Google进行登录，但现在您只尝试使用GitHub进行登录。

## 解决方案

您可以通过同时使用GitHub和Google进行登录来解决此错误，然后，如果您想要将Google帐户与Streamlit Community Cloud解绑，您可以仅通过Google进行注销。
