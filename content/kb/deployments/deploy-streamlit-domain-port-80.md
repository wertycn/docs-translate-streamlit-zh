---
title: 如何将Streamlit部署到域名上，以便在常规端口（即端口80）上运行？
slug: /knowledge-base/deploy/deploy-streamlit-domain-port-80
---

# 如何将Streamlit部署到域名上，以便在常规端口（即端口80）上运行？

## 问题

您希望将Streamlit应用程序部署到一个域名上，以便在端口80上运行。

## 解决方案

- 您应该使用**反向代理**来将来自Web服务器（如[Apache](https://httpd.apache.org/)或[Nginx](https://www.nginx.com/)）的请求转发到运行Streamlit应用程序的端口。您可以通过几种不同的方式来实现这一点。最简单的方式是[将发送到您的域的所有请求转发](https://discuss.streamlit.io/t/permission-denied-in-ec2-port-80/798/3)，以便您的Streamlit应用程序显示为您网站的内容。

- 另一种方法是配置您的Web服务器将请求转发到指定的子文件夹（例如_http://awesomestuff.net/streamlitapp_），以在同一域上为不同的Streamlit应用程序提供服务，就像[这个Nginx的示例配置](https://discuss.streamlit.io/t/how-to-use-streamlit-with-nginx/378/7)一样，由Streamlit社区的成员提交。

相关论坛帖子：

- https://discuss.streamlit.io/t/permission-denied-in-ec2-port-80/798/3
- https://discuss.streamlit.io/t/how-to-use-streamlit-with-nginx/378/7
