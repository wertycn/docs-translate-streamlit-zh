---
title: 登录后出现“嗯，这不应该发生”的消息
slug: /knowledge-base/deploy/huh-this-isnt-supposed-to-happen-message-after-trying-to-log-in
---

# 登录后出现“嗯，这不应该发生”的消息

本文帮助解决GitHub和Streamlit社区云之间的电子邮件不匹配引起的登录问题。

## 问题

您在登录到Streamlit社区云帐户后看到以下消息：

![哦。这不应该发生的消息](/images/knowledge-base/huh-this-isnt-supposed-to-happen.png)

这个消息通常表示我们的系统将您的GitHub用户名与与您当前登录的电子邮件地址不同的电子邮件地址关联起来。

## 解决方案

别担心 - 您只需执行以下操作：

1. 完全退出Streamlit Community Cloud（通过您的电子邮件和GitHub帐户）。
2. 首先使用您的电子邮件账户登录（您可以通过["继续使用Google登录"](/streamlit-community-cloud/get-started#sign-in-with-google)或["继续使用电子邮件登录"](/knowledge-base/deploy/sign-in-without-sso)来进行登录）。
3. 使用您的[GitHub账户](/streamlit-community-cloud/get-started#sign-in-with-email)登录。
