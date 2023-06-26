---
slug: /knowledge-base/deploy/does-streamlit-support-wsgi-protocol
title: Does Streamlit support the WSGI Protocol? (aka Can I deploy Streamlit with
  gunicorn?)
---

# Streamlit是否支持WSGI协议？（也就是说，我可以使用gunicorn部署Streamlit吗？）

## 问题

您不确定您的Streamlit应用程序是否可以使用gunicorn进行部署。

## 解决方案

Streamlit目前不支持WSGI协议，因此无法使用gunicorn等方式部署Streamlit。请查看[这个论坛帖子](https://discuss.streamlit.io/t/how-do-i-set-the-server-to-0-0-0-0-for-deployment-using-docker/216)，了解其他用户是如何解决这个问题的。