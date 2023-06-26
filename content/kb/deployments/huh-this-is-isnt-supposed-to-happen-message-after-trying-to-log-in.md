---
slug: /knowledge-base/deploy/huh-this-isnt-supposed-to-happen-message-after-trying-to-log-in
title: Huh. This is isn't supposed to happen message after trying to log in
---

# 哎呀。登录后出现了这不应该发生的消息

本文帮助解决由于GitHub和Streamlit社区云之间的电子邮件不匹配而导致的登录问题。

## 问题

在登录到您的Streamlit社区云账户后，您会看到以下消息:

![哎呀。这不应该发生的消息](/images/knowledge-base/huh-this-isnt-supposed-to-happen.png)

这个消息通常表示我们的系统将您的GitHub用户名与与您当前登录的电子邮件地址不同的电子邮件地址关联起来。

## 解决方案

不用担心 - 您只需要：

1. 完全注销Streamlit社区云（通过您的电子邮件和GitHub帐户）。
2. 首先使用您的电子邮件帐户登录（您可以通过["继续使用Google登录"](/streamlit-community-cloud/get-started#sign-in-with-google)或["继续使用电子邮件登录"](/knowledge-base/deploy/sign-in-without-sso)来完成）。
3. 使用您的 [GitHub 帐户登录](/streamlit-community-cloud/get-started#sign-in-with-email)。