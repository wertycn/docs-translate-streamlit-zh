---
description: st.altair_chart displays a chart using the Altair library.
slug: /library/api-reference/charts/st.altair_chart
title: st.altair_chart
---

<Autofunction function="streamlit.altair_chart" />

### 主题

默认情况下，Altair图表使用Streamlit主题进行显示。该主题简洁、用户友好，并且融入了Streamlit的颜色调色板。另一个好处是，您的图表能更好地与应用程序的其他设计相融合。

从Streamlit 1.16.0版本开始，您可以通过`theme="streamlit"`关键字参数来使用Streamlit主题。如果要禁用它并使用Altair的原生主题，可以使用`theme=None`。

让我们来看一个使用Streamlit主题和原生Altair主题的图表示例：

```python
import altair as alt
from vega_datasets import data

source = data.cars()

chart = alt.Chart(source).mark_circle().encode(
    x='Horsepower',
    y='Miles_per_Gallon',
    color='Origin',
).interactive()

tab1, tab2 = st.tabs(["Streamlit theme (default)", "Altair native theme"])

with tab1:
    # Use the Streamlit theme.
    # This is the default. So you can also omit the theme argument.
    st.altair_chart(chart, theme="streamlit", use_container_width=True)
with tab2:
    # Use the native Altair theme.
    st.altair_chart(chart, theme=None, use_container_width=True)
```

点击下面的交互式应用程序中的选项卡，可以查看启用和禁用Streamlit主题的图表。

<Cloud src="https://doc-altair-chart.streamlit.app/?embed=true" height="500" />

如果您想知道自己的自定义配置是否仍然会被考虑到，不用担心！您仍然可以对图表配置进行更改。换句话说，尽管我们现在默认启用了Streamlit主题，但您可以使用自定义颜色或字体进行覆盖。例如，如果您希望图表线条是绿色而不是默认的红色，您可以这样做！

以下是一个Altair图表的示例，其中进行了手动颜色传递并进行了反映：

<折叠标题="查看代码">

```python
import altair as alt
import streamlit as st
from vega_datasets import data

source = data.seattle_weather()

scale = alt.Scale(
    domain=["sun", "fog", "drizzle", "rain", "snow"],
    range=["#e7ba52", "#a7a7a7", "#aec7e8", "#1f77b4", "#9467bd"],
)
color = alt.Color("weather:N", scale=scale)

# We create two selections:
# - a brush that is active on the top panel
# - a multi-click that is active on the bottom panel
brush = alt.selection_interval(encodings=["x"])
click = alt.selection_multi(encodings=["color"])

# Top panel is scatter plot of temperature vs time
points = (
    alt.Chart()
    .mark_point()
    .encode(
        alt.X("monthdate(date):T", title="Date"),
        alt.Y(
            "temp_max:Q",
            title="Maximum Daily Temperature (C)",
            scale=alt.Scale(domain=[-5, 40]),
        ),
        color=alt.condition(brush, color, alt.value("lightgray")),
        size=alt.Size("precipitation:Q", scale=alt.Scale(range=[5, 200])),
    )
    .properties(width=550, height=300)
    .add_selection(brush)
    .transform_filter(click)
)

# Bottom panel is a bar chart of weather type
bars = (
    alt.Chart()
    .mark_bar()
    .encode(
        x="count()",
        y="weather:N",
        color=alt.condition(click, color, alt.value("lightgray")),
    )
    .transform_filter(brush)
    .properties(
        width=550,
    )
    .add_selection(click)
)

chart = alt.vconcat(points, bars, data=source, title="Seattle Weather: 2012-2015")

tab1, tab2 = st.tabs(["Streamlit theme (default)", "Altair native theme"])

with tab1:
    st.altair_chart(chart, theme="streamlit", use_container_width=True)
with tab2:
    st.altair_chart(chart, theme=None, use_container_width=True)
```

</Collapse>

请注意，即使启用了Streamlit主题，自定义颜色仍会反映在图表中👇

<Cloud src="https://doc-altair-custom-colors.streamlit.app/?embed=true" height="675" />

要了解更多使用和不使用Streamlit主题的Altair图表示例，请访问[altair.streamlit.app](https://altair.streamlit.app)。

### 注释图表

Altair 还允许您使用文本、图像和表情符号对图表进行注释。您可以通过创建[分层图表](https://altair-viz.github.io/user_guide/compound_charts.html#layered-charts)来实现这一点，这样可以在两个不同的图表之上叠加显示。其思想是使用第一个图表来展示数据，使用第二个图表来展示注释。然后，可以使用 `+` 运算符将第二个图表叠加在第一个图表之上，创建一个分层图表。

让我们通过一个示例来演示如何在时间序列图上添加文本和表情符号的注释。

#### 第一步：创建基础图表

在这个示例中，我们创建一个时间序列图来跟踪股票价格的变化。该图表是交互式的，并包含一个多行提示框。点击[这里](https://altair-viz.github.io/gallery/multiline_tooltip.html)了解更多关于Altair中多行提示框的信息。

首先，我们导入所需的库，并使用 [`vega_datasets`](https://pypi.org/project/vega-datasets/) 包加载示例股票数据集:

```python
import altair as alt
import pandas as pd
import streamlit as st
from vega_datasets import data

# We use @st.cache_data to keep the dataset in cache
@st.cache_data
def get_data():
    source = data.stocks()
    source = source[source.date.gt("2004-01-01")]
    return source

source = get_data()
```

接下来，我们定义一个名为 `get_chart()` 的函数，用于创建一个带有多行工具提示的交互式时间序列图，图表的 x 轴表示日期，y 轴表示股票价格。

然后，我们调用 `get_chart()` 函数，将股票价格的数据框作为输入，并返回一个图表对象。这将成为我们基础图表，在[第二步](/library/api-reference/charts/st.altair_chart#step-2-annotate-the-chart)中我们将在其上面叠加注释。

```python
# Define the base time-series chart.
def get_chart(data):
    hover = alt.selection_single(
        fields=["date"],
        nearest=True,
        on="mouseover",
        empty="none",
    )

    lines = (
        alt.Chart(data, title="Evolution of stock prices")
        .mark_line()
        .encode(
            x="date",
            y="price",
            color="symbol",
        )
    )

    # Draw points on the line, and highlight based on selection
    points = lines.transform_filter(hover).mark_circle(size=65)

    # Draw a rule at the location of the selection
    tooltips = (
        alt.Chart(data)
        .mark_rule()
        .encode(
            x="yearmonthdate(date)",
            y="price",
            opacity=alt.condition(hover, alt.value(0.3), alt.value(0)),
            tooltip=[
                alt.Tooltip("date", title="Date"),
                alt.Tooltip("price", title="Price (USD)"),
            ],
        )
        .add_selection(hover)
    )
    return (lines + points + tooltips).interactive()

chart = get_chart(source)
```

#### 第二步：标注图表

现在我们已经有了显示数据的第一个图表，我们可以在图表上添加文本和表情符号进行标注。让我们在特定的兴趣点上叠加⬇️表情符号在时间序列图上。我们希望用户将鼠标悬停在⬇️表情符号上时，能够看到相关的标注文本。

为了简单起见，让我们在四个特定的日期上进行标注，并将标注的高度设置为固定值 `10`。

<Tip>

您可以通过使用Streamlit小部件的输出替换硬编码的值来改变注释的水平和垂直位置！点击[这里](/library/api-reference/charts/st.altair_chart#interactive-example)跳转到下面的实时示例，并通过与Streamlit小部件进行交互来培养对注释的理想水平和垂直位置的直觉。

</Tip>

为了实现这一目标，我们创建了一个名为 `annotations_df` 的数据帧，其中包含日期、注释文本和注释的高度。

```python
# Add annotations
ANNOTATIONS = [
    ("Mar 01, 2008", "Pretty good day for GOOG"),
    ("Dec 01, 2007", "Something's going wrong for GOOG & AAPL"),
    ("Nov 01, 2008", "Market starts again thanks to..."),
    ("Dec 01, 2009", "Small crash for GOOG after..."),
]
annotations_df = pd.DataFrame(ANNOTATIONS, columns=["date", "event"])
annotations_df.date = pd.to_datetime(annotations_df.date)
annotations_df["y"] = 10
```

使用这个数据帧，我们可以创建一个散点图，其中 x 轴表示日期，y 轴表示注释的高度。具体日期和高度的数据点使用 ⬇ 表情符号表示，使用 Altair 的 `mark_text()` [mark](https://altair-viz.github.io/user_guide/marks.html)。

当用户将鼠标悬停在⬇表情符号上方时，注释文本将显示为工具提示。通过使用Altair的`encode()`方法，将包含注释文本的`event`列映射到图表的可视属性⬇来实现此效果。

```python
annotation_layer = (
    alt.Chart(annotations_df)
    .mark_text(size=20, text="⬇", dx=-8, dy=-10, align="left")
    .encode(
        x="date:T",
        y=alt.Y("y:Q"),
        tooltip=["event"],
    )
    .interactive()
)
```

最后，我们使用`+`运算符将注释叠加在基础图表上，从而创建最终的分层图表！🎈

```python
st.altair_chart(
    (chart + annotation_layer).interactive(),
    use_container_width=True
)
```

使用图片替代表情符号，请将包含`.mark_text()`的行替换为`.mark_image()`，并将下面的`image_url`替换为图片的URL地址：

```python
mark.image(image_url)
```

```python
.mark_image(
    width=12,
    height=12,
    url="image_url",
)
```

#### 交互示例

现在，您已经学会了如何注释图表，可以尽情发挥了！我们扩展了上面的示例，让您可以交互地粘贴您最喜欢的表情符号，并使用Streamlit小部件设置其在图表上的位置。👇

<Cloud src="https://example-time-series-annotation.streamlit.app/?embed=true" height="700" />