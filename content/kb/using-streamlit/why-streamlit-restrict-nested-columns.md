---
slug: /knowledge-base/using-streamlit/why-streamlit-restrict-nested-columns
title: Why does Streamlit restrict nested st.columns?
---

# 为什么Streamlit限制嵌套的`st.columns`？

从1.18.0版本开始，Streamlit允许在其他`st.columns`内嵌套[`st.columns`](/library/api-reference/layout/st.columns)，但有以下限制：

- 在应用程序的主区域，列可以嵌套一层。
- 在侧边栏中，列不能嵌套。

这些限制是为了让Streamlit应用在所有设备尺寸上都看起来很好。多次嵌套列通常会导致不良的用户界面。
你可能能够在一个屏幕尺寸上使其看起来很好，但是当不同屏幕上的用户查看应用程序时，他们会有糟糕的体验。一些列会很小，其他列会过长，复杂的布局会显得不协调。Streamlit会尽其所能自动调整元素的大小，以在不同设备上呈现良好的效果，而不需要开发人员的帮助。但对于具有多层嵌套的复杂布局，这是不可能的。

我们始终致力于改进布局选项！因此，如果您有一个需要更复杂布局的用例，请[打开一个GitHub问题](https://github.com/streamlit/streamlit/issues)，最好附上您想要做的草图。