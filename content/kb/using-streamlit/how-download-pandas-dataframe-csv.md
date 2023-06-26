---
slug: /knowledge-base/using-streamlit/how-download-pandas-dataframe-csv
title: How to download a Pandas DataFrame as a CSV?
---

# 如何将Pandas DataFrame下载为CSV文件？

使用内置于Streamlit的[`st.download_button`](/library/api-reference/widgets/st.download_button)小部件。查看一个[示例应用程序](https://streamlit-release-demos-0-88streamlit-app-0-88-v8ram3.streamlit.app/)，演示了如何使用`st.download_button`下载常见文件格式。

## 示例用法

```python
import streamlit as st
import pandas as pd

df = pd.read_csv("dir/file.csv")

@st.experimental_memo
def convert_df(df):
   return df.to_csv(index=False).encode('utf-8')


csv = convert_df(df)

st.download_button(
   "Press to Download",
   csv,
   "file.csv",
   "text/csv",
   key='download-csv'
)
```

附加资源:

- <https://blog.streamlit.io/0-88-0-release-notes/>
- <https://streamlit-release-demos-0-88streamlit-app-0-88-v8ram3.streamlit.app/>
- <https://docs.streamlit.io/library/api-reference/widgets/st.download_button>