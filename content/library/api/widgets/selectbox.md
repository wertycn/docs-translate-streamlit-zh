---
description: st.selectbox displays a select widget.
slug: /library/api-reference/widgets/st.selectbox
title: st.selectbox
---

`<Autofunction function="streamlit.selectbox" />`

<br />

选择小部件可以通过`label_visibility`参数自定义如何隐藏标签。如果为"hidden"，标签不会显示，但在小部件上方仍然有空白空间（相当于`label=""`）。如果为"collapsed"，标签和空白空间都会被移除。默认值是"visible"。选择小部件还可以通过`disabled`参数来禁用：

```python
import streamlit as st

# Store the initial value of widgets in session state
if "visibility" not in st.session_state:
    st.session_state.visibility = "visible"
    st.session_state.disabled = False

col1, col2 = st.columns(2)

with col1:
    st.checkbox("Disable selectbox widget", key="disabled")
    st.radio(
        "Set selectbox label visibility 👉",
        key="visibility",
        options=["visible", "hidden", "collapsed"],
    )

with col2:
    option = st.selectbox(
        "How would you like to be contacted?",
        ("Email", "Home phone", "Mobile phone"),
        label_visibility=st.session_state.visibility,
        disabled=st.session_state.disabled,
    )
```

<云 src="https://doc-selectbox1.streamlit.app/?embed=true" 高度="300" />