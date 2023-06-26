---
description: st.radio displays a radio button widget.
slug: /library/api-reference/widgets/st.radio
title: st.radio
---

\<Autofunction function="streamlit.radio" />

<br />

通过 `label_visibility` 参数，小部件可以自定义如何隐藏其标签。如果设置为 "hidden"，标签将不显示，但仍会在小部件上方留出空白空间（相当于 `label=""`）。如果设置为 "collapsed"，则标签和空白空间都会被移除。默认值为 "visible"。单选按钮还可以通过 `disabled` 参数禁用，并通过 `horizontal` 参数水平排列：

```python
import streamlit as st

# Store the initial value of widgets in session state
if "visibility" not in st.session_state:
    st.session_state.visibility = "visible"
    st.session_state.disabled = False
    st.session_state.horizontal = False

col1, col2 = st.columns(2)

with col1:
    st.checkbox("Disable radio widget", key="disabled")
    st.checkbox("Orient radio options horizontally", key="horizontal")

with col2:
    st.radio(
        "Set label visibility 👇",
        ["visible", "hidden", "collapsed"],
        key="visibility",
        label_visibility=st.session_state.visibility,
        disabled=st.session_state.disabled,
        horizontal=st.session_state.horizontal,
    )
```

<云 src="https://doc-radio1.streamlit.app/?embed=true" height="300" />

### 特色视频

来看看我们关于如何使用Streamlit的核心功能之一——单选按钮的视频吧！🔘

<YouTube videoId="CVHIMGVAzwA" />

在下面的视频中，我们将进一步学习如何结合[按钮](/library/api-reference/widgets/st.button)、[复选框](/library/api-reference/widgets/st.checkbox)和单选按钮！

<YouTube videoId="EnXJBsCIl_A" />