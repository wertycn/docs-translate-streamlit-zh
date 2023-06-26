---
slug: /knowledge-base/deploy/deploy-streamlit-domain-port-80
title: How do I deploy Streamlit on a domain so it appears to run on a regular port
  (i.e. port 80)?
---

# 如何在域名上部署Streamlit，以便它在常规端口（即端口80）上运行？

## 问题

您希望在域名上部署Streamlit应用程序，以便它在端口80上运行。

## 解决方案

- 您应该使用**反向代理**将来自像[Apache](https://httpd.apache.org/)或[Nginx](https://www.nginx.com/)这样的Web服务器的请求转发到您的Streamlit应用程序运行的端口。您可以通过几种不同的方式来实现这一点。最简单的方法是[将发送到您域名的所有请求转发](https://discuss.streamlit.io/t/permission-denied-in-ec2-port-80/798/3)，以便您的Streamlit应用程序显示为您网站的内容。

- 另一种方法是配置您的web服务器将请求转发到指定的子文件夹（例如_http://awesomestuff.net/streamlitapp_），以便在同一域名下的不同Streamlit应用程序之间进行区分，就像这个[nginx的示例配置](https://discuss.streamlit.io/t/how-to-use-streamlit-with-nginx/378/7)一样，由Streamlit社区成员提交。

相关论坛帖子：

- https://discuss.streamlit.io/t/permission-denied-in-ec2-port-80/798/3
- https://discuss.streamlit.io/t/how-to-use-streamlit-with-nginx/378/7