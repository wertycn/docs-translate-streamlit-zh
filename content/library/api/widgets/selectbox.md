---
标题：st.selectbox
链接：/library/api-reference/widgets/st.selectbox
描述：st.selectbox显示一个选择小部件。

<Autofunction function="streamlit.selectbox" />

<br />

选择性的小部件可以使用`label_visibility`参数自定义如何隐藏它们的标签。如果设置为"hidden"，标签将不显示，但是在小部件上方仍然有空白空间（相当于`label=""`）。如果设置为"collapsed"，则标签和空白空间都会被删除。默认值为"visible"。选择性的小部件还可以使用`disabled`参数进行禁用。

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

<Cloud src="https://doc-selectbox1.streamlit.app/?embed=true" height="300" />
