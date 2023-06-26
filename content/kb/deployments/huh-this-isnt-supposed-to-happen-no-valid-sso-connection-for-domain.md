---
slug: /knowledge-base/deploy/huh-this-isnt-supposed-to-happen-no-valid-sso-connection-for-domain
title: Huh. This isn't supposed to happen. No valid SSO connection for domain
---

# 哎呀。这不应该发生。没有有效的SSO连接到域

本文档将帮助您解决在使用SSO登录Streamlit Community Cloud时出现的"没有有效的SSO连接"错误。

## 问题

当尝试使用SSO（SAML认证）登录Streamlit Community Cloud帐户时，您会看到以下屏幕：

![没有有效的SSO连接到域](/images/knowledge-base/no-valid-sso-connection-for-this-domain.png)

这个消息意味着您过去曾使用GitHub和Google同时登录，但现在您只想使用GitHub登录。

## 解决方案

您可以通过同时使用GitHub和Google登录来解决此错误。如果您希望将Google帐户与Streamlit社区云解绑，您可以只使用Google退出登录。