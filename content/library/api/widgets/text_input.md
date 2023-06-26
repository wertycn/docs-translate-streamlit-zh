---
description: st.text_input displays a single-line text input widget.
slug: /library/api-reference/widgets/st.text_input
title: st.text_input
---

<Autofunction function="streamlit.text_input" />

<br />

文本输入小部件可以使用`label_visibility`参数自定义标签的隐藏方式。如果设置为“hidden”，标签将不会显示，但在小部件上方仍然会保留空白空间（相当于`label=""`）。如果设置为“collapsed”，则标签和空白空间都将被移除。默认值为“visible”。文本输入小部件还可以使用`disabled`参数禁用，并可以使用`placeholder`参数在文本输入为空时显示可选的占位文本。

```python
import streamlit as st

# Store the initial value of widgets in session state
if "visibility" not in st.session_state:
    st.session_state.visibility = "visible"
    st.session_state.disabled = False

col1, col2 = st.columns(2)

with col1:
    st.checkbox("Disable text input widget", key="disabled")
    st.radio(
        "Set text input label visibility 👉",
        key="visibility",
        options=["visible", "hidden", "collapsed"],
    )
    st.text_input(
        "Placeholder for the other text input widget",
        "This is a placeholder",
        key="placeholder",
    )

with col2:
    text_input = st.text_input(
        "Enter some text 👇",
        label_visibility=st.session_state.visibility,
        disabled=st.session_state.disabled,
        placeholder=st.session_state.placeholder,
    )

    if text_input:
        st.write("You entered: ", text_input)
```

<Cloud src="https://doc-text-input1.streamlit.app/?embed=true" height="400" />