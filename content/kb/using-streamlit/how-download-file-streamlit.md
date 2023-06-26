---
slug: /knowledge-base/using-streamlit/how-download-file-streamlit
title: How to download a file in Streamlit?
---

# 如何在Streamlit中下载文件？

使用Streamlit内置的[`st.download_button`](/library/api-reference/widgets/st.download_button)小部件。查看一个示例应用程序，演示如何使用`st.download_button`来下载常见的文件格式。

## 示例用法

```python
import streamlit as st

# Text files

text_contents = '''
Foo, Bar
123, 456
789, 000
'''

# Different ways to use the API

st.download_button('Download CSV', text_contents, 'text/csv')
st.download_button('Download CSV', text_contents)  # Defaults to 'text/plain'

with open('myfile.csv') as f:
   st.download_button('Download CSV', f)  # Defaults to 'text/plain'

# ---
# Binary files

binary_contents = b'whatever'

# Different ways to use the API

st.download_button('Download file', binary_contents)  # Defaults to 'application/octet-stream'

with open('myfile.zip', 'rb') as f:
   st.download_button('Download Zip', f, file_name='archive.zip')  # Defaults to 'application/octet-stream'

# You can also grab the return value of the button,
# just like with any other button.

if st.download_button(...):
   st.write('Thanks for downloading!')
```

附加资源:

- <https://blog.streamlit.io/0-88-0-release-notes/>
- <https://streamlit-release-demos-0-88streamlit-app-0-88-v8ram3.streamlit.app/>
- <https://docs.streamlit.io/library/api-reference/widgets/st.download_button>