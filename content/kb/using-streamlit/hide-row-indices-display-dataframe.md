---
title: 在显示数据框时隐藏行索引
slug: /knowledge-base/using-streamlit/hide-row-indices-displaying-dataframe
---

# 在显示数据框时隐藏行索引

## 概述

Streamlit提供两种显示数据框的方式：使用[`st.table()`](/library/api-reference/data/st.table)作为静态表格，使用[`st.dataframe()`](/library/api-reference/data/st.dataframe)作为交互式表格。

这两个选项在最左侧列中显示行索引。为了看到它们的效果，让我们使用`st.table()`和`st.dataframe()`来显示一个具有随机条目的数据帧：

```python
import streamlit as st
import pandas as pd
import numpy as np

df = pd.DataFrame(
    np.random.randn(10, 5),
    columns=("col %d" % i for i in range(5)))

# Display a static table
st.table(df)

# Display an interactive table
st.dataframe(df)
```

注意：行索引显示在`col0`列的左侧：👇

![显示数据帧](/images/knowledge-base/display-dataframe.png)

要隐藏包含行索引的列，您可以使用CSS选择器来修改列的可见性。在显示数据帧之前，您必须使用 `st.markdown()` 注入适当的CSS，并设置 `unsafe_allow_html=True`。

现在，您已经对如何隐藏行索引有了概念性的理解，让我们在代码中实现它！

## 使用st.table隐藏行索引

在使用st.table时，可以隐藏行索引。

```python
import streamlit as st
import pandas as pd
import numpy as np

df = pd.DataFrame(
    np.random.randn(10, 5),
    columns=("col %d" % i for i in range(5)))

# CSS to inject contained in a string
hide_table_row_index = """
            <style>
            thead tr th:first-child {display:none}
            tbody th {display:none}
            </style>
            """

# Inject CSS with Markdown
st.markdown(hide_table_row_index, unsafe_allow_html=True)

# Display a static table
st.table(df)
```

![隐藏数据框索引](/images/knowledge-base/hide-dataframe-index.png)

## 使用`st.dataframe`隐藏行索引

<警告>

下面的解决方法对于`streamlit>=1.10.0`不起作用。

</警告>

```python
import streamlit as st
import pandas as pd
import numpy as np

df = pd.DataFrame(
    np.random.randn(10, 5),
    columns=("col %d" % i for i in range(5)))

# CSS to inject contained in a string
hide_dataframe_row_index = """
            <style>
            .row_heading.level0 {display:none}
            .blank {display:none}
            </style>
            """

# Inject CSS with Markdown
st.markdown(hide_dataframe_row_index, unsafe_allow_html=True)

# Display an interactive table
st.dataframe(df)
```

![隐藏表格索引](/images/knowledge-base/hide-table-index.png)
