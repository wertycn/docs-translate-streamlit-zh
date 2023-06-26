---
title: 向表格或图表附加数据
slug: /knowledge-base/using-streamlit/append-data-table-chart
---

# 向表格或图表附加数据

在Streamlit中，您不仅可以替换应用程序中的整个元素，还可以修改这些元素背后的数据。以下是如何操作的步骤：

```python
import numpy as np
import time

# Get some data.
data = np.random.randn(10, 2)

# Show the data as a chart.
chart = st.line_chart(data)

# Wait 1 second, so the change is clearer.
time.sleep(1)

# Grab some more data.
data2 = np.random.randn(10, 2)

# Append the new data to the existing chart.
chart.add_rows(data2)
```
