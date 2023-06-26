---
标题：st.altair_chart
别名：/library/api-reference/charts/st.altair_chart
描述：st.altair_chart使用Altair库显示图表。

<Autofunction function="streamlit.altair_chart" />

### 主题

默认情况下，Altair图表使用Streamlit主题显示。该主题设计时尚、用户友好，并融合了Streamlit的颜色调色板。另一个好处是，您的图表能够更好地与应用程序的其他设计相融合。

Streamlit主题可以从Streamlit 1.16.0版本开始通过`theme="streamlit"`关键字参数使用。要禁用它并使用Altair的原生主题，请改用`theme=None`。

让我们来看一个使用Streamlit主题和Altair原生主题的图表示例:

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

点击下面的交互式应用程序中的选项卡，查看启用和禁用Streamlit主题的图表。

<Cloud src="https://doc-altair-chart.streamlit.app/?embed=true" height="500" />

如果您想知道您自己的自定义配置是否仍然会被考虑到，请不用担心！您仍然可以对图表配置进行更改。换句话说，尽管我们现在默认启用Streamlit主题，但您可以使用自定义颜色或字体来覆盖它。例如，如果您希望图表线条是绿色而不是默认的红色，您可以这样做！

这是一个Altair图表的示例，其中进行了手动颜色传递并得到了反映：

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

注意，即使启用了Streamlit主题，自定义颜色仍会反映在图表中👇

<Cloud src="https://doc-altair-custom-colors.streamlit.app/?embed=true" height="675" />

要查看更多使用Streamlit主题和不使用Streamlit主题的Altair图表示例，请访问[altair.streamlit.app](https://altair.streamlit.app)。

### 注释图表

Altair还允许您使用文本、图像和表情符号对图表进行注释。您可以通过创建[层叠图表](https://altair-viz.github.io/user_guide/compound_charts.html#layered-charts)来实现这一点，这样您可以将两个不同的图表叠加在一起。其思想是使用第一个图表显示数据，使用第二个图表显示注释。然后，可以使用`+`运算符将第二个图表叠加在第一个图表上，创建一个层叠图表。

让我们通过一个示例来演示如何在时间序列图上使用文本和表情符号进行标注。

#### 步骤1：创建基本图表

在这个示例中，我们创建一个时间序列图来跟踪股票价格的演变。该图表是交互式的，并包含一个多行提示框。点击[这里](https://altair-viz.github.io/gallery/multiline_tooltip.html)了解更多关于Altair中多行提示框的信息。

首先，我们导入所需的库，并使用[`vega_datasets`](https://pypi.org/project/vega-datasets/)包加载示例股票数据集：

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

接下来，我们定义一个名为`get_chart()`的函数，用于创建具有多行工具提示的交互式时间序列图表，其中x轴表示日期，y轴表示股票价格。

然后，我们调用`get_chart()`函数，将股票价格数据框作为输入，并返回一个图表对象。这将是我们在[第2步](/library/api-reference/charts/st.altair_chart#step-2-annotate-the-chart)中将要叠加注释的基础图表。

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

#### 第二步：给图表添加注释

现在我们已经有了展示数据的第一个图表，我们可以用文本和表情符号对其进行注释。让我们将⬇️表情符号叠加在时间序列图上的特定兴趣点上。我们希望用户将鼠标悬停在⬇️表情符号上，以查看相关的注释文本。

为了简单起见，让我们在四个特定的日期上进行注释，并将注释的高度设置为恒定值`10`。

<Tip>

您可以通过使用Streamlit小部件的输出来更改注释的水平和垂直位置，而不是硬编码的值！点击[这里](/library/api-reference/charts/st.altair_chart#interactive-example)跳转到下面的实时示例，并通过使用Streamlit小部件来培养对注释的理想水平和垂直位置的直觉。

为了实现这一目标，我们创建了一个名为`annotations_df`的数据帧，其中包含日期、注释文本和注释的高度信息：

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

使用这个数据框，我们可以创建一个散点图，其中x轴表示日期，y轴表示注释的高度。特定日期和高度上的数据点由⬇️表情符号表示，使用Altair的`mark_text()` [mark](https://altair-viz.github.io/user_guide/marks.html)。

当用户将鼠标悬停在⬇表情符号上方时，注释文本将显示为工具提示。此效果是通过使用Altair的`encode()`方法将包含注释文本的`event`列映射到绘图的可视属性⬇实现的。

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

最后，我们使用`+`运算符将注释叠加在基础图表之上，从而创建最终的分层图表！🎈

```python
st.altair_chart(
    (chart + annotation_layer).interactive(),
    use_container_width=True
)
```

使用图片代替表情符号，请将包含`.mark_text()`的行替换为`.mark_image()`，并将下面的`image_url`替换为图片的URL：


```python
.mark_image(
    width=12,
    height=12,
    url="image_url",
)
```

#### 交互式示例

现在你已经学会了如何注释图表，一切都取决于你的想象力！我们对上面的示例进行了扩展，让你可以通过Streamlit小部件交互式地粘贴你最喜欢的表情符号，并设置它在图表上的位置。👇

<Cloud src="https://example-time-series-annotation.streamlit.app/?embed=true" height="700" />
