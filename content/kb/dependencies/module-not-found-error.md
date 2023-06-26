---
title: ModuleNotFoundError 未找到模块
slug: /knowledge-base/dependencies/module-not-found-error
---

# ModuleNotFoundError: No module named

## 问题

当您在[Streamlit Community Cloud](https://streamlit.io/cloud)上部署应用程序时，您会收到错误消息 `ModuleNotFoundError: No module named`。

## 解决方案

当您在Streamlit Community Cloud上导入一个在您的requirements文件中未包含的模块时，就会出现这个错误。任何不随[标准Python安装](https://docs.python.org/3/py-modindex.html)分发的外部[Python依赖项](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)都应该包含在您的requirements文件中。

例如，如果您的要求文件中没有包含`scikit-learn`，并且在应用程序中没有导入`import sklearn`，则会出现`ModuleNotFoundError: No module named 'sklearn'`的错误。

相关的论坛帖子：

- https://discuss.streamlit.io/t/getting-error-modulenotfounderror-no-module-named-beautifulsoup/9126
- https://discuss.streamlit.io/t/modulenotfounderror-no-module-named-vega-datasets/16354
