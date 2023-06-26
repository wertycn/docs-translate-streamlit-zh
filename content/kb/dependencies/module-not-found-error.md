---
slug: /knowledge-base/dependencies/module-not-found-error
title: ModuleNotFoundError No module named
---

# ModuleNotFoundError: 找不到模块

## 问题

当您在[Streamlit Community Cloud](https://streamlit.io/cloud)上部署应用程序时，您会收到`ModuleNotFoundError: 找不到模块`的错误。

## 解决方案

当您在Streamlit Community Cloud上导入一个未包含在您的要求文件中的模块时，就会出现此错误。任何不在[标准Python安装](https://docs.python.org/3/py-modindex.html)中分发的外部[Python依赖项](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)都应该包含在您的要求文件中。

例如，如果您的要求文件中没有包含`scikit-learn`，并且在您的应用程序中没有导入`sklearn`，则会出现`ModuleNotFoundError: No module named 'sklearn'`的错误。

相关的论坛帖子：

- https://discuss.streamlit.io/t/getting-error-modulenotfounderror-no-module-named-beautifulsoup/9126
- https://discuss.streamlit.io/t/modulenotfounderror-no-module-named-vega-datasets/16354