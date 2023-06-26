---
title: Streamlit组件与基本Streamlit包中提供的功能有何不同？
slug: /knowledge-base/components/how-streamlit-components-differ-base-package
---

# Streamlit组件与基本Streamlit包中提供的功能有何不同？

- Streamlit组件被包装在一个iframe中，这使您可以使用任何喜欢的Web技术在iframe中进行任何操作。

- 组件和Streamlit之间有一个严格的消息协议，这使得组件可以作为小部件。由于Streamlit组件被包装在iframe中，它们无法修改其父级的DOM（即Streamlit报告），这确保了即使是用户编写的组件，Streamlit也始终是安全的。
