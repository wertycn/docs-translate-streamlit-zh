---
title: 如何在不同子域名上部署多个Streamlit应用程序？
slug: /knowledge-base/deploy/deploy-multiple-streamlit-apps-different-subdomains
---

# 如何在不同子域名上部署多个Streamlit应用程序？

## 问题

您想在不同的子域名上部署多个Streamlit应用程序。

## 解决方案

与在更常见的端口（如80）上运行Streamlit应用程序一样，子域名由像Apache或Nginx这样的Web服务器处理：

- 在具有公共IP地址的机器上设置Web服务器，然后使用DNS服务器将所有所需的子域指向您的Web服务器的IP地址

- 配置您的Web服务器，将对每个子域的请求路由到您的Streamlit应用程序运行的不同端口上

举个例子，假设你有两个名为`Calvin`和`Hobbes`的Streamlit应用程序。应用程序`Calvin`正在端口**8501**上运行。你设置应用程序`Hobbes`在端口**8502**上运行。然后，你的Web服务器将被设置为在子域名`calvin.somedomain.com`和`hobbes.subdomain.com`上监听请求，并将请求分别路由到端口**8501**和**8502**。

请参考以下两个教程，它们分别介绍了如何使用Apache2和Nginx设置Web服务器以将子域名重定向到不同的端口:

- [Apache2子域名](https://stackoverflow.com/questions/8541182/apache-redirect-to-another-port)
- [NGinx子域名](https://gist.github.com/soheilhy/8b94347ff8336d971ad0)
