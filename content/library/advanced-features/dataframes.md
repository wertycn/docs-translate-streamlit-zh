---
title: 数据帧
slug: /library/advanced-features/dataframes
---

# 数据帧

数据框是以表格形式显示和编辑数据的一种很好的方式。与Pandas数据框和其他表格数据结构一起工作是数据科学工作流程的关键。如果开发人员和数据科学家想要在Streamlit中显示这些数据，他们有多个选项：`st.dataframe`和`st.data_editor`。如果您只想在表格样式的用户界面中显示数据，那么使用[st.dataframe](/library/api-reference/data/st.dataframe)是一个不错的选择。如果您想要交互式地编辑数据，请使用[st.data_editor](/library/api-reference/data/st.data_editor)。我们在下面的章节中探讨了每个选项的用例和优势。

## 使用 `st.dataframe` 显示数据框

Streamlit可以通过 `st.dataframe` 在表格样式的用户界面中显示数据框：

```python
import streamlit as st
import pandas as pd

df = pd.DataFrame(
    [
        {"command": "st.selectbox", "rating": 4, "is_widget": True},
        {"command": "st.balloons", "rating": 5, "is_widget": False},
        {"command": "st.time_input", "rating": 3, "is_widget": True},
    ]
)

st.dataframe(df, use_container_width=True)
```

<Cloud src="https://doc-dataframe-basic.streamlit.app/?embed=true" height="300px"/>

## 附加的UI功能

`st.dataframe` 还通过在内部使用 [glide-data-grid](https://github.com/glideapps/glide-data-grid) 提供了一些附加功能：

- **列排序**：通过点击列头来对列进行排序。
- **列调整大小**：通过拖动和放下列头边框来调整列的大小。
- **表格调整大小**：通过拖动和放下右下角来调整表格的大小。
- **搜索**: 通过点击表格，在搜索栏中使用快捷键（`⌘ Cmd + F` 或 `Ctrl + F`）进行搜索，并使用搜索栏过滤数据。
- **复制到剪贴板**: 选择一个或多个单元格，将它们复制到剪贴板，并粘贴到您喜欢的电子表格软件中。

![dataframe-ui.gif](/images/dataframe-ui.gif)

使用上一节的嵌入式应用程序尝试所有其他的UI功能。

除了Pandas DataFrames外，`st.dataframe`还支持其他常见的Python类型，例如list、dict或numpy数组。它还支持[Snowpark](https://docs.snowflake.com/en/developer-guide/snowpark/index)和[PySpark](https://spark.apache.org/docs/latest/api/python/) DataFrames，这使您可以惰性评估和从数据库中提取数据。这对于处理大型数据集非常有用。

## 使用st.data_editor编辑数据

Streamlit通过`st.data_editor`命令支持可编辑的数据框。在[st.data_editor](/library/api-reference/data/st.data_editor)中可以查看其API。它以表格的形式显示数据框，类似于`st.dataframe`。但与`st.dataframe`不同的是，该表格是可编辑的！用户可以点击单元格并对其进行编辑。编辑后的数据将返回到Python端。下面是一个示例：

```python
df = pd.DataFrame(
    [
        {"command": "st.selectbox", "rating": 4, "is_widget": True},
        {"command": "st.balloons", "rating": 5, "is_widget": False},
        {"command": "st.time_input", "rating": 3, "is_widget": True},
    ]
)

df = load_data()
edited_df = st.data_editor(df) # 👈 An editable dataframe

favorite_command = edited_df.loc[edited_df["rating"].idxmax()]["command"]
st.markdown(f"Your favorite command is **{favorite_command}** 🎈")
```

<折叠标题="查看交互式应用程序">

<Cloud src="https://doc-data-editor.streamlit.app/?embed=true" height="300px"/>

</折叠>

通过双击任何单元格来尝试它。您会注意到您可以编辑所有单元格的值。尝试编辑评分列中的值，并观察底部的文本输出如何变化：

![data-editor-editing.gif](/images/data-editor-editing.gif)

`st.data_editor` 还支持一些额外的功能：

- [复制和粘贴支持](#copy-and-paste-support) 从 Excel 和 Google Sheets 到应用中，以及从应用到 Excel 和 Google Sheets。
- [添加和删除行](#add-and-delete-rows)。在调用 `st.data_editor` 时，可以通过将 `num_rows= "dynamic"` 设置为实现此功能。这将允许用户根据需要添加和删除行。
- [访问已编辑的数据](#access-edited-data)。通过会话状态只访问单个编辑，而不是整个已编辑数据结构。
- [批量编辑](#bulk-edits)（类似于Excel，只需拖动手柄即可编辑相邻单元格）。
- [自动输入验证](#automatic-input-validation)，强大的数据类型支持。例如，无法在数字单元格中输入字母，并且还有许多可配置的输入验证选项，例如最小/最大值。
- [编辑常见的数据结构](#edit-common-data-structures)，如列表、字典、NumPy ndarray等。

### 支持复制和粘贴

数据编辑器支持从Google Sheets、Excel、Notion和许多其他类似工具中粘贴表格数据。您还可以在`st.data_editor`实例之间复制粘贴数据。这个功能是由[Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)支持的，对于需要在多个平台上处理数据的用户来说，可以节省大量时间。要尝试它：

1. 将数据从[此Google Sheets文档](https://docs.google.com/spreadsheets/d/1Z0zd-5dF_HfqUaDDq4BWAOnsdlGCjkbTNwDZMBQ1dOY/edit?usp=sharing)复制到剪贴板。
2. 选择下面表格中`name`列中的任意单元格，并粘贴进去（通过`ctrl/cmd + v`）。

<展开 title="查看交互式应用程序">

<Cloud src="https://doc-data-editor-clipboard.streamlit.app/?embed=true" height="400px"/>

</展开>

![data-editor-clipboard.gif](/images/data-editor-clipboard.gif)

<注释>

每个粘贴的数据单元格都将被单独评估，并在数据与列类型兼容时插入到单元格中。例如，将非数值文本数据粘贴到数字列中将被忽略。

</注意事项>

您是否注意到，尽管初始数据框只有五行，但从电子表格中粘贴所有这些行会向数据框添加额外的行？ 👀 让我们在下一节中找出这是如何工作的。

<提示>

如果您在应用程序中嵌入了iframe，如果您想使用复制粘贴功能，您需要允许iframe访问剪贴板。为此，请给予iframe[`clipboard-write`](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard/write)和[`clipboard-read`](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard/read)权限。例如：

```javascript
<iframe allow="clipboard-write;clipboard-read;" ... src="https://your-app-url"></iframe>
```

作为开发者，在使用TLS时，请确保应用程序使用有效且受信任的证书进行服务。如果用户在复制和粘贴数据时遇到问题，请引导他们检查浏览器是否已激活了对Streamlit应用程序的剪贴板访问权限，可以在提示时或通过浏览器的站点设置中进行设置。

</Tip>

### 添加和删除行

通过`st.data_editor`，查看器可以通过表格界面添加或删除行。可以通过将`num_rows`参数设置为`"dynamic"`来激活此模式。例如：

```python
edited_df = st.data_editor(df, num_rows="dynamic")
```

- 要添加新行，请滚动到最底部的行，然后在任何单元格上点击“+”符号。
- 要删除行，请选择一个或多个行，并按键盘上的`delete`键。

<折叠标题="查看交互式应用">

<云端源="https://doc-data-editor-clipboard.streamlit.app/?embed=true" 高度="400px"/>

</折叠>

![data-editor-add-delete.gif](/images/data-editor-add-delete.gif)

### 访问已编辑的数据

有时候，了解哪些单元格已经被更改比获取整个编辑后的数据框更方便。Streamlit通过使用[会话状态](https://docs.streamlit.io/library/advanced-features/session-state)来实现这一点。如果设置了`key`参数，Streamlit将会将对数据框所做的任何更改存储在会话状态中。

以下代码片段展示了如何使用会话状态来访问更改后的数据：

```python
st.data_editor(df, key="data_editor") # 👈 Set a key
st.write("Here's the session state:")
st.write(st.session_state["data_editor"]) # 👈 Access the edited data
```

在这段代码片段中，`key`参数被设置为`"data_editor"`。对于`st.data_editor`实例中的数据所做的任何更改都将被Streamlit跟踪，并存储在会话状态下的`"data_editor"`键下。

创建数据编辑器之后，使用`st.write(st.session_state["data_editor"])`将会把session状态中`"data_editor"`键的内容打印到屏幕上。这样可以让您看到对原始数据帧所做的更改，而无需返回整个数据帧。

当处理大型数据帧并且只需要知道哪些单元格发生了更改时，而不是整个编辑后的数据帧时，这非常有用。

<折叠标题="查看交互式应用程序">

<Cloud src="https://doc-data-editor-changed.streamlit.app/?embed=true" height="700px"/>

</Collapse>

将我们之前学到的所有知识应用到上面的嵌入式应用程序中。尝试编辑单元格、添加新行和删除行。

![data-editor-session-state.gif](/images/data-editor-session-state.gif)

请注意，对表格的编辑会在会话状态中反映出来：当您进行任何编辑时，将触发重新运行，通过`st.data_editor`的键控小部件状态将编辑内容发送到后端。它的小部件状态是一个包含三个属性的JSON对象：**edited_rows**，**added_rows**和**deleted_rows**。

<警告>

在1.23.0版本中，从`st.experimental_data_editor`迁移到`st.data_editor`时，数据编辑器在`st.session_state`中的表示方式发生了变化。现在，`edited_cells`字典被称为`edited_rows`，并且使用了不同的格式（`{0: {"column name": "edited value"}}`，而不是`{"0:1": "edited value"}`）。如果您的应用程序在`st.experimental_data_editor`和`st.session_state`的组合中使用了，请根据需要调整代码。"

</Warning>

- `edited_rows` 是一个包含所有编辑的字典。键是基于零的行索引，值是将列名映射到编辑的字典（例如 `{0: {"col1": ..., "col2": ...}}`）。
- `added_rows` 是一个新添加行的列表。每个值都是一个具有与上述相同格式的字典（例如 `[{"col1": ..., "col2": ...}]`）。
- `deleted_rows` 是一个已从表格中删除的行号列表（例如 `[0, 2]`）。

### 批量编辑

数据编辑器包含一个批量编辑单元格的功能。类似于Excel，您可以拖动一个手柄来选择一组单元格，并进行批量编辑它们的值。您甚至可以在电子表格软件中应用常用的[键盘快捷键](https://github.com/glideapps/glide-data-grid/blob/main/packages/core/API.md#keybindings)。当您需要对多个单元格进行相同的更改时，而不是逐个编辑每个单元格时，这非常有用。

![data-editor-bulk-editing.gif](/images/data-editor-bulk-editing.gif)

### 自动输入验证

数据编辑器包括自动输入验证，以帮助在编辑单元格时防止错误。例如，如果您有一个包含数字数据的列，输入字段将自动限制用户只能输入数字数据。这有助于防止用户意外输入非数字值可能导致的错误。可以通过[列配置API](/library/api-reference/data/st.column_config)进行额外的输入验证配置。例如，对于[文本列](/library/api-reference/data/st.column_config/st.column_config.textcolumn)，可以设置`max_chars`和`validate`模式，对于[数字列](/library/api-reference/data/st.column_config/st.column_config.numbercolumn)，可以设置`min_value`、`max_value`或`step`。您还可以将所有可编辑列类型的`required`设置为`True`，以禁止将值设置为`None`。

### 编辑常见的数据结构

编辑不仅适用于Pandas DataFrames！您还可以编辑列表、元组、集合、字典、NumPy数组或Snowpark和PySpark DataFrames。大多数数据类型将以其原始格式返回。但是某些类型（例如Snowpark和PySpark）将被转换为Pandas DataFrames。要了解所有支持的类型，请阅读[st.data_editor](/library/api-reference/data/st.data_editor) API。

例如，您可以轻松让用户向列表中添加项目：

```python
edited_list = st.data_editor(["red", "green", "blue"], num_rows= "dynamic")
st.write("Here are all the colors you entered:")
st.write(edited_list)
```

或者是NumPy数组：

```python
import numpy as np

st.data_editor(np.array([
	["st.text_area", "widget", 4.92],
	["st.markdown", "element", 47.22]
]))
```

或记录的列表:

```python
st.data_editor([
    {"name": "st.text_area", "type": "widget"},
    {"name": "st.markdown", "type": "element"},
])
```

或者字典和许多其他类型！

```python
st.data_editor({
	"st.text_area": "widget",
	"st.markdown": "element"
})
```

## 配置列

您可以通过 `st.dataframe` 和 `st.data_editor` 使用 [列配置API](/library/api-reference/data/st.column_config) 来配置列的显示和编辑行为。我们已经开发了这个API，让您可以在数据框和数据编辑器的列中添加图片、图表和可点击的URL。此外，您还可以使单个列可编辑，将列设置为分类列并指定其可以接受的选项，隐藏数据框的索引等等。

### 列配置

在使用Streamlit处理数据时，[`st.column_config`](/library/api-reference/data/st.column_config) 类是一个强大的工具，用于配置数据的显示和交互。它专门为 [`st.dataframe`](/library/api-reference/data/st.dataframe) 和 [`st.data_editor`](/library/api-reference/data/st.data_editor) 中的 `column_config` 参数设计，提供了一套方法来根据各种数据类型对列进行定制 - 包括简单的文本和数字，列表，URL，图像等等。

无论是将时间数据转换为用户友好的格式，还是利用图表和进度条进行更清晰的数据可视化，列配置不仅提供了丰富的数据查看体验，还确保您拥有以您想要的方式呈现和交互数据的工具。

<TileContainer>
<RefCard href="/library/api-reference/data/st.column_config/st.column_config.column">
![screenshot](/images/api/column_config.column.jpg)

#### Column

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

配置数字列。

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

#### 下拉框列

配置一个下拉框列。

```python
SelectboxColumn("App Category", options=["🤖 LLM", "📈 Data Viz"])
```

</RefCard>

<RefCard href="/library/api-reference/data/st.column_config/st.column_config.datetimecolumn">
<Image pure alt="屏幕截图" src="/images/api/column_config.datetimecolumn.jpg" />

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

配置一个链接列。

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

<Important>

在接下来的两周内，我们将发布关于如何配置列的深入文档和博客文章。请密切关注本页面和[Streamlit博客](https://blog.streamlit.io/)的更新。

</Important>

今天您可以使用Pandas的技巧来将列呈现为复选框、选择框，并更改列的类型，而无需使用列配置API。我们将在下面的章节中探讨这些技术。

### 布尔列（复选框）

要将列渲染为复选框，并在 `st.dataframe` 和 `st.data_editor` 中将复选框设置为可点击的复选框，可以将 Pandas 列的类型设置为 `bool`。

下面是一个创建包含布尔值的 Pandas DataFrame 的示例。当我们使用 `st.dataframe` 显示它时，布尔值将被渲染为复选框，其中 `True` 和 `False` 值分别被选中和取消选中。

```python
import pandas as pd

# create a dataframe with a boolean column
df = pd.DataFrame({"A": [True, False, True, False]})

# show the dataframe with checkboxes
st.dataframe(df)
```

![data-editor-dataframe-boolean.gif](/images//data-editor-dataframe-boolean.gif)

请注意，您无法从前端更改它们的值。为了让用户能够选中和取消选中值，我们使用`st.data_editor`来显示数据帧：

```python
import pandas as pd

# create a dataframe with a boolean column
df = pd.DataFrame({"A": [True, False, True, False]})

# show the data editor with checkboxes
st.data_editor(df)
```

![data-editor-boolean.gif](/images/data-editor-boolean.gif)

### 分类列（下拉选择框）

要使用 `st.data_editor` 将列呈现为下拉选择框，请将 Pandas 列的类型设置为 `category`：

```python
import pandas as pd

df = pd.DataFrame(
    {"command": ["st.selectbox", "st.slider", "st.balloons", "st.time_input"]}
)
df["command"] = df["command"].astype("category")

edited_df = st.data_editor(df)
```

在某些情况下，您可能希望用户选择原始Pandas DataFrame中没有的类别。假设我们使用上面的 `df`。目前，`command` 列可以有四个唯一值。如果我们希望用户看到额外选项，比如 `st.button` 和 `st.radio`，应该怎么办呢？

要将额外的类别添加到选择中，请使用 [pandas.Series.cat.add_categories](https://pandas.pydata.org/docs/reference/api/pandas.Series.cat.add_categories.html)：

```python
import pandas as pd

df = pd.DataFrame(
    {"command": ["st.selectbox", "st.slider", "st.balloons", "st.time_input"]}
)
df["command"] = (
    df["command"].astype("category").cat.add_categories(["st.button", "st.radio"])
)

edited_df = st.data_editor(df)
```

![data-editor-categorical.gif](/images/data-editor-categorical.gif)

<!-- ### 更改列类型

要更改列的类型，您可以更改基础Pandas DataFrame列的类型。例如，假设您有一个仅包含整数的列，但希望用户能够添加带有小数的数字。要实现这一点，只需将Pandas DataFrame列的类型更改为`float`，如下所示:

```python
import pandas as pd

import streamlit as st

# create a dataframe with an integer column
df = pd.DataFrame({"A": [1, 2, 3, 4]})

# unable to add float values to the column
edited_df = st.data_editor(df)

# cast the column to float
df["A"] = df["A"].astype("float")

# able to add float values to the column
edited_df = st.data_editor(df)
```

在第一个数据编辑器实例中，您不能将十进制值添加到任何条目中。但是，在将列`A`转换为`float`类型之后，我们可以将值作为浮点数进行编辑：

![data-editor-change-type.gif](/images/data-editor-change-type.gif) -->

## 处理大型数据集

`st.dataframe`和`st.data_editor`通过使用高性能的glide-data-grid库和HTML canvas实现，理论上可以处理包含数百万行的表格。但是，应用程序实际能够处理的最大数据量将取决于其他几个因素，包括：

1. WebSocket消息的最大大小：Streamlit的WebSocket消息可以通过`server.maxMessageSize` [配置选项](https://docs.streamlit.io/library/advanced-features/configuration#view-all-configuration-options)进行配置，该选项限制了通过WebSocket连接一次可以传输的数据量。
2. 服务器内存：您的应用程序能够处理的数据量还取决于服务器上可用的内存量。如果超过服务器的内存限制，应用程序可能会变得缓慢或无响应。
3. 用户浏览器的内存：由于所有数据都需要传输到用户的浏览器进行渲染，用户设备上可用的内存量也会影响应用程序的性能。如果浏览器的内存超过限制，可能会导致崩溃或无响应。

除了这些因素之外，网络连接缓慢也会显著减慢处理大数据集的应用程序的速度。

当处理超过150,000行的大数据集时，Streamlit会应用额外的优化并禁用列排序。这可以帮助减少一次需要处理的数据量，并提高应用程序的性能。

## 限制

虽然Streamlit的数据编辑功能提供了很多功能，但也有一些限制需要注意：

- 编辑功能仅适用于一组有限的列类型（[TextColumn](/library/api-reference/data/st.column_config/st.column_config.textcolumn), [NumberColumn](/library/api-reference/data/st.column_config/st.column_config.numbercolumn), [LinkColumn](/library/api-reference/data/st.column_config/st.column_config.linkcolumn), [CheckboxColumn](/library/api-reference/data/st.column_config/st.column_config.checkboxcolumn), [SelectboxColumn](/library/api-reference/data/st.column_config/st.column_config.selectboxcolumn), [DateColumn](/library/api-reference/data/st.column_config/st.column_config.datecolumn), [TimeColumn](/library/api-reference/data/st.column_config/st.column_config.timecolumn) 和 [DatetimeColumn](/library/api-reference/data/st.column_config/st.column_config.datetimecolumn)）。我们正在积极努力支持其他列类型的编辑，如图像、列表和图表。
- Pandas DataFrame 的编辑仅支持以下索引类型: `RangeIndex`, (字符串) `Index`, `Float64Index`, `Int64Index` 和 `UInt64Index`。
- 删除行或搜索数据等某些操作只能通过键盘快捷键触发。

我们正在努力解决上述限制，并将在未来的版本中进行更新，敬请关注更新。
