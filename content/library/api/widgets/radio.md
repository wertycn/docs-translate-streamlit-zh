---
标题: st.radio
链接: /library/api-reference/widgets/st.radio
描述: st.radio显示一个单选按钮小部件。
---

<Autofunction function="streamlit.radio" />

<br />

小部件可以使用`label_visibility`参数自定义标签的隐藏方式。如果设置为"hidden"，标签将不会显示，但在小部件上方仍然有空白空间（相当于`label=""`）。如果设置为"collapsed"，则标签和空白空间都将被移除。默认值为"visible"。单选按钮还可以使用`disabled`参数禁用，并使用`horizontal`参数水平排列：

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

<Cloud src="https://doc-radio1.streamlit.app/?embed=true" height="300" />

### 精选视频

查看我们的视频，了解如何使用Streamlit的核心功能之一 - 单选按钮！🔘

<YouTube videoId="CVHIMGVAzwA" />

在下面的视频中，我们将进一步学习如何组合[按钮](/library/api-reference/widgets/st.button)、[复选框](/library/api-reference/widgets/st.checkbox)和单选按钮！

<YouTube videoId="EnXJBsCIl_A" />
