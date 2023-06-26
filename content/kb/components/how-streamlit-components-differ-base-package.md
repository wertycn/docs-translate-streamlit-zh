---
slug: /knowledge-base/components/how-streamlit-components-differ-base-package
title: How do Streamlit Components differ from functionality provided in the base
  Streamlit package?
---

# Streamlit组件与基本的Streamlit包提供的功能有什么区别？

- Streamlit组件被包装在一个iframe中，这使您可以使用任何您喜欢的Web技术，在iframe内部做任何您想做的事情。

- 组件和Streamlit之间存在严格的消息协议，这使得组件能够充当小部件。由于Streamlit组件被包装在iframe中，它们无法修改其父级的DOM（也就是Streamlit报告），这确保了即使使用用户编写的组件，Streamlit始终是安全的。