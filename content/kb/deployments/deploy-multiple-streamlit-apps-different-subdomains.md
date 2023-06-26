---
slug: /knowledge-base/deploy/deploy-multiple-streamlit-apps-different-subdomains
title: How can I deploy multiple Streamlit apps on different subdomains?
---

# 如何在不同子域上部署多个Streamlit应用？

## 问题

您想在不同的子域上部署多个Streamlit应用。

## 解决方案

与在常见端口（如80）上运行Streamlit应用类似，子域由像Apache或Nginx这样的Web服务器处理：

- 在具有公共IP地址的计算机上设置一个Web服务器，然后使用DNS服务器将所有期望的子域指向您的Web服务器的IP地址

- 配置您的Web服务器，将每个子域名的请求路由到不同的端口上，这些端口是您的Streamlit应用程序正在运行的端口

例如，假设您有两个Streamlit应用程序，分别称为`Calvin`和`Hobbes`。 应用程序`Calvin`正在端口**8501**上运行。 您设置应用程序`Hobbes`在端口**8502**上运行。 然后，您的Web服务器将被配置为在子域名`calvin.somedomain.com`和`hobbes.subdomain.com`上"监听"请求，并将请求分别路由到端口**8501**和**8502**。

请查看以下两个关于Apache2和Nginx的教程，它们介绍了如何设置一个Web服务器来将子域名重定向到不同的端口：

- [Apache2子域名教程](https://stackoverflow.com/questions/8541182/apache-redirect-to-another-port)
- [NGinx子域名教程](https://gist.github.com/soheilhy/8b94347ff8336d971ad0)