---
标题: 如何隐藏应用程序的汉堡菜单？
链接: /knowledge-base/using-streamlit/how-hide-hamburger-menu-app
---

# 如何隐藏应用程序的汉堡菜单？

## 概述

Streamlit允许开发者通过[`st.set_page_config()`](/library/api-reference/utilities/st.set_page_config)来配置他们的汉堡菜单，使其更加用户友好。虽然您可以使用`st.set_page_config()`配置菜单项，但是没有原生支持来隐藏/移除应用程序中的菜单。

<div style={{ marginBottom: '-3em' }}>
<Flex>
<Image caption="1：汉堡菜单出现在应用程序的右上方。" src="/images/knowledge-base/hamburger-menu-app.png" />
</Flex>
</div>

## 解决方案

但是，您可以使用一个非官方的 CSS hack 来隐藏您的应用程序中的菜单。要这样做，请在您的应用程序中包含以下代码片段：

```python
import streamlit as st

hide_menu_style = """
        <style>
        #MainMenu {visibility: hidden;}
        </style>
        """
st.markdown(hide_menu_style, unsafe_allow_html=True)
```

汉堡菜单将不再出现在您的应用程序的右上方，如下图所示！ 🎈

<div style={{ marginBottom: '-3em' }}>
<Flex>
<Image caption="2: 用非官方的CSS hack隐藏汉堡菜单。" src="/images/knowledge-base/hamburger-menu-removed.png" />
</Flex>
</div>

## 相关资源：

- [隐藏汉堡菜单的能力＃395](https://github.com/streamlit/streamlit/issues/395)
- [如何在生产环境中隐藏/移除菜单？](https://discuss.streamlit.io/t/how-do-i-hide-remove-the-menu-in-production/362/12)
- [用于移除汉堡菜单的非官方CSS技巧](https://www.youtube.com/watch?v=0_HlInz6HuM)
- [官方支持的配置汉堡菜单的方法](/library/api-reference/utilities/st.set_page_config)
- [Streamlit的当前路线图](https://roadmap.streamlit.io/)
