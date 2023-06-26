---
title: Streamlit是否支持WSGI协议？（也就是说，我可以使用gunicorn部署Streamlit吗？）
slug: /knowledge-base/deploy/does-streamlit-support-wsgi-protocol
---

# Streamlit是否支持WSGI协议？（也就是说，我可以使用gunicorn部署Streamlit吗？）

## 问题

您不确定您的Streamlit应用程序是否可以使用gunicorn进行部署。

## 解决方案

Streamlit目前不支持WSGI协议，因此目前无法使用（例如）gunicorn部署Streamlit。请查看[有关以类似gunicorn方式部署Streamlit的论坛帖子](https://discuss.streamlit.io/t/how-do-i-set-the-server-to-0-0-0-0-for-deployment-using-docker/216)，了解其他用户是如何完成这个任务的。
