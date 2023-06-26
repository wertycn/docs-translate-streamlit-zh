---
title: 如何获取使用st.file_uploader上传的文件的文件名？
slug: /knowledge-base/using-streamlit/retrieve-filename-uploaded
---

# 如何获取使用st.file_uploader上传的文件的文件名？

如果您上传了一个单个文件（即 `accept_multiple_files=False`），可以通过在返回的UploadedFile对象上使用`.name`属性来获取文件名：

```python
import streamlit as st

uploaded_file = st.file_uploader("Upload a file")

if uploaded_file:
   st.write("Filename: ", uploaded_file.name)
```

如果您上传多个文件（即 `accept_multiple_files=True`），可以通过在返回的列表中的每个UploadedFile对象上使用`.name`属性来检索各个文件的文件名：

```python
import streamlit as st

uploaded_files = st.file_uploader("Upload multiple files", accept_multiple_files=True)

if uploaded_files:
   for uploaded_file in uploaded_files:
       st.write("Filename: ", uploaded_file.name)
```

相关论坛帖子：

- https://discuss.streamlit.io/t/is-it-possible-to-get-uploaded-file-file-name/7586
