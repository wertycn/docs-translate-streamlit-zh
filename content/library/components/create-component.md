---
title: 创建组件
slug: /library/components/create
---

# 创建组件

<Note>

如果您只对**使用Streamlit组件**感兴趣，那么您可以跳过本节，转到[Streamlit组件库](https://streamlit.io/components)查找和安装由社区创建的组件！

</Note>

开发人员可以编写 JavaScript 和 HTML 的 "组件"，这些组件可以在 Streamlit 应用中进行渲染。Streamlit 组件可以从 Streamlit Python 脚本接收数据，也可以向其发送数据。

Streamlit 组件可以扩展基本的 Streamlit 包提供的功能。使用 Streamlit 组件为您的用例创建所需的功能，然后将其封装在一个 Python 包中，并与更广泛的 Streamlit 社区共享！

**您可以创建的Streamlit组件类型包括:**

- 自定义现有的Streamlit元素和小部件，比如`st.slider`或`st.file_uploader`。
- 通过包装现有的React.js、Vue.js或其他JavaScript小部件工具包，创建全新的Streamlit元素和小部件。
- 渲染具有输出HTML方法的Python对象，例如IPython [`__repr_html__`](https://ipython.readthedocs.io/en/stable/config/integrating.html#rich-display)。
- 针对常用的网络功能，提供了方便的函数，例如[GitHub gists和Pastebin](https://github.com/randyzwitch/streamlit-embedcode)。

请查看Streamlit工程师Tim Conkling的这些Streamlit组件教程视频，以开始学习：

## 第一部分：设置和架构

<YouTube videoId="BuD3gILJW-Q" />

## 第二部分：创建滑动条小部件

<YouTube videoId="QjccJl_7Jco" />
