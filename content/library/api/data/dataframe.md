---
description: st.dataframe displays a dataframe as an interactive table.
slug: /library/api-reference/data/st.dataframe
title: st.dataframe
---

<提示>

本页面只包含关于`st.dataframe` API的信息。如果您想深入了解如何使用数据框，请阅读[数据框](/library/advanced-features/dataframes)。如果您希望让用户可以交互式地编辑数据框，请查看[`st.data_editor`](/library/api-reference/data/st.data_editor)。

</提示>

<自动函数 函数="streamlit.dataframe" />

<br />

`st.dataframe`支持`use_container_width`参数，可以使其宽度占满整个容器：

```python
import pandas as pd
import streamlit as st

# Cache the dataframe so it's only loaded once
@st.cache_data
def load_data():
    return pd.DataFrame(
        {
            "first column": [1, 2, 3, 4],
            "second column": [10, 20, 30, 40],
        }
    )

# Boolean to resize the dataframe, stored as a session state variable
st.checkbox("Use container width", value=False, key="use_container_width")

df = load_data()

# Display the dataframe and allow the user to stretch the dataframe
# across the full width of the container, based on the checkbox value
st.dataframe(df, use_container_width=st.session_state.use_container_width)
```

<Cloud src="https://doc-dataframe2.streamlit.app/?embed=true" height="350" />

### 列配置

在使用Streamlit处理数据时，[`st.column_config`](/library/api-reference/data/st.column_config) 类是一种强大的工具，用于配置数据的显示和交互。它专门设计用于 [`st.dataframe`](/library/api-reference/data/st.dataframe) 和 [`st.data_editor`](/library/api-reference/data/st.data_editor) 的 `column_config` 参数，提供了一套方法来根据不同的数据类型来定制您的列 - 从简单的文本和数字到列表、URL、图像等等。

无论是将时间数据转换成用户友好的格式，还是利用图表和进度条进行更清晰的数据可视化，列配置不仅为用户提供了丰富的数据查看体验，还确保您具备以您想要的方式呈现和交互数据的工具。

<TileContainer>
<RefCard href="/library/api-reference/data/st.column_config/st.column_config.column">
![screenshot](/images/api/column_config.column.jpg)

#### 列配置

配置一个通用列。

```python
Column("Streamlit Widgets", width="medium", help="Streamlit **widget** commands 🎈")
```

</RefCard>
<RefCard href="/library/api-reference/data/st.column_config/st.column_config.textcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.textcolumn.jpg" />

#### 文本列

配置一个文本列。

```python
TextColumn("Widgets", max_chars=50, validate="^st\.[a-z_]+$")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.numbercolumn">
<Image pure alt="screenshot" src="/images/api/column_config.numbercolumn.jpg" />

#### 数字列

配置一个数字列。

```python
NumberColumn("Price (in USD)", min_value=0, format="$%d")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.checkboxcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.checkboxcolumn.jpg" />

#### 复选框列

配置一个复选框列。

```python
CheckboxColumn("Your favorite?", help="Select your **favorite** widgets")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.selectboxcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.selectboxcolumn.jpg" />

#### 选择框列

配置一个选择框列。

```python
SelectboxColumn("App Category", options=["🤖 LLM", "📈 Data Viz"])
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.datetimecolumn">
<Image pure alt="screenshot" src="/images/api/column_config.datetimecolumn.jpg" />

#### 日期时间列

配置一个日期时间列。

```python
DatetimeColumn("Appointment", min_value=datetime(2023, 6, 1), format="D MMM YYYY, h:mm a")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.datecolumn">
<Image pure alt="screenshot" src="/images/api/column_config.datecolumn.jpg" />

#### 日期列

配置一个日期列。

```python
DateColumn("Birthday", max_value=date(2005, 1, 1), format="DD.MM.YYYY")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.timecolumn">
<Image pure alt="screenshot" src="/images/api/column_config.timecolumn.jpg" />

#### 时间列

配置一个时间列。

```python
TimeColumn("Appointment", min_value=time(8, 0, 0), format="hh:mm a")
```

</RefCard>
<RefCard href="/library/api-reference/data/st.column_config/st.column_config.listcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.listcolumn.jpg" />

#### 列表列

配置一个列表列。

```python
ListColumn("Sales (last 6 months)", width="medium")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.linkcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.linkcolumn.jpg" />

#### 链接列

配置链接列。

```python
LinkColumn("Trending apps", max_chars=100, validate="^https://.*$")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.imagecolumn">
<Image pure alt="screenshot" src="/images/api/column_config.imagecolumn.jpg" />

#### 图像列

配置图像列。

```python
ImageColumn("Preview Image", help="The preview screenshots")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.linechartcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.linechartcolumn.jpg" />

#### 折线图列

配置一个折线图列。

```python
LineChartColumn("Sales (last 6 months)" y_min=0, y_max=100)
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.barchartcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.barchartcolumn.jpg" />

#### 柱状图列

配置一个柱状图列。

```python
BarChartColumn("Marketing spend" y_min=0, y_max=100)
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.progresscolumn">
<Image pure alt="screenshot" src="/images/api/column_config.progresscolumn.jpg" />

#### Progress column

Configure a progress column.

```python
ProgressColumn("Sales volume", min_value=0, max_value=1000, format="$%f")
```

</RefCard>

</TileContainer>

### 交互性

使用`st.dataframe`显示的数据帧具有以下交互功能：

- **列排序**：通过点击列标题进行排序。
- **列调整大小**：通过拖动和放置列头边框来调整列的大小。
- **表格（高度、宽度）调整大小**：通过拖动和放置表格右下角来调整表格的大小。
- **搜索**：通过点击表格，在搜索栏中使用快捷键（`⌘ Cmd + F` 或 `Ctrl + F`）进行搜索，并使用搜索栏过滤数据。
- **复制到剪贴板**：选择一个或多个单元格，将其复制到剪贴板，然后粘贴到您喜欢的电子表格软件中。

![数据框界面演示图](/images/dataframe-ui.gif)