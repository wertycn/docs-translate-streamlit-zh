---
标题：st.dataframe
网址：/library/api-reference/data/st.dataframe
描述：st.dataframe 将数据帧显示为交互式表格。

<Tip>

本页面仅包含关于 `st.dataframe` API 的信息。如果您想更深入地了解如何处理数据帧，请阅读 [数据帧](/library/advanced-features/dataframes)。如果您想让用户可以交互地编辑数据帧，请查看 [`st.data_editor`](/library/api-reference/data/st.data_editor)。

</Tip>

`st.dataframe`函数支持`use_container_width`参数，可以使其占满整个容器宽度:

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

在使用Streamlit处理数据时，[`st.column_config`](/library/api-reference/data/st.column_config)类是一个用于配置数据显示和交互的强大工具。该类专门为[`st.dataframe`](/library/api-reference/data/st.dataframe)和[`st.data_editor`](/library/api-reference/data/st.data_editor)的`column_config`参数设计，它提供了一套方法，可以根据不同的数据类型来自定义列 - 从简单的文本和数字到列表、URL、图片等等。

无论是将时间数据转换为用户友好的格式，还是利用图表和进度条进行更清晰的数据可视化，列配置不仅为用户提供了丰富的数据查看体验，而且确保您拥有以您想要的方式呈现和与数据进行交互的工具。

<TileContainer>
<RefCard href="/library/api-reference/data/st.column_config/st.column_config.column">
![屏幕截图](/images/api/column_config.column.jpg)

#### 列

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
<Image pure alt="屏幕截图" src="/images/api/column_config.checkboxcolumn.jpg" />

#### 复选框列

配置一个复选框列。

```python
CheckboxColumn("Your favorite?", help="Select your **favorite** widgets")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.selectboxcolumn">
<Image pure alt="screenshot" src="/images/api/column_config.selectboxcolumn.jpg" />

#### 选择框列

配置选择框列。

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
<Image pure alt="截图" src="/images/api/column_config.datecolumn.jpg" />

#### 日期列

配置一个日期列。

```python
DateColumn("Birthday", max_value=date(2005, 1, 1), format="DD.MM.YYYY")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.timecolumn">
<Image pure alt="screenshot" src="/images/api/column_config.timecolumn.jpg" />

#### 时间列

配置时间列。

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

#### 图片列

配置图片列。

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
<Image pure alt="截图" src="/images/api/column_config.barchartcolumn.jpg" />

#### 条形图列

配置一个条形图列。

```python
BarChartColumn("Marketing spend" y_min=0, y_max=100)
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.progresscolumn">
<Image pure alt="screenshot" src="/images/api/column_config.progresscolumn.jpg" />

#### 进度列

配置一个进度列。

```python
ProgressColumn("Sales volume", min_value=0, max_value=1000, format="$%f")
```

</RefCard>

</TileContainer>

### 交互性

使用`st.dataframe`显示的数据框具有以下交互功能：

- **列排序**：通过单击列标题进行排序。
- **列调整大小**：通过拖动和放置列标题边界来调整列大小。
- **表格（高度、宽度）调整大小**：通过拖动和放置表格的右下角来调整表格大小。
- **搜索**：通过单击表格，在搜索栏中使用热键（`⌘ Cmd + F` 或 `Ctrl + F`）来搜索数据，并使用搜索栏来筛选数据。
- **复制到剪贴板**：选择一个或多个单元格，将它们复制到剪贴板，并粘贴到您喜欢的电子表格软件中。

<Image src="/images/dataframe-ui.gif" />
