---
slug: /knowledge-base/components/not-possibe-streamlit-components
title: What types of things aren't possible with Streamlit Components?
---

# Streamlit组件有哪些不可能的事情？

由于每个Streamlit组件都被加载到自己的沙箱iframe中，这意味着在组件上存在一些限制：

- **无法与其他组件通信**：组件无法包含（或以其他方式与）其他组件进行通信，因此无法使用组件来构建类似`grid_layout`的内容。
- **无法修改CSS**：组件无法修改Streamlit应用程序使用的CSS，因此无法创建类似于`dark_mode`的效果。
- **无法添加/删除元素**：组件无法添加或删除Streamlit应用程序的其他元素，因此无法创建类似于`remove_streamlit_hamburger_menu`的效果。