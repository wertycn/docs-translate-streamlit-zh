---
标题: st.plotly_chart
slug: /library/api-reference/charts/st.plotly_chart
描述: st.plotly_chart显示一个交互式的Plotly图表。

<Autofunction function="streamlit.plotly_chart" />

### 主题

默认情况下，Plotly图表使用Streamlit的主题显示。这个主题时尚、用户友好，并且融合了Streamlit的颜色调色板。另一个好处是，您的图表能更好地与应用程序的其他设计融合在一起。

从Streamlit 1.16.0开始，可以通过使用`theme="streamlit"`关键字参数来使用Streamlit主题。要禁用它并使用Plotly的原生主题，可以使用`theme=None`。

让我们看一个使用Streamlit主题和原生Plotly主题的图表示例：

```python
import plotly.express as px
import streamlit as st

df = px.data.gapminder()

fig = px.scatter(
    df.query("year==2007"),
    x="gdpPercap",
    y="lifeExp",
    size="pop",
    color="continent",
    hover_name="country",
    log_x=True,
    size_max=60,
)

tab1, tab2 = st.tabs(["Streamlit theme (default)", "Plotly native theme"])
with tab1:
    # Use the Streamlit theme.
    # This is the default. So you can also omit the theme argument.
    st.plotly_chart(fig, theme="streamlit", use_container_width=True)
with tab2:
    # Use the native Plotly theme.
    st.plotly_chart(fig, theme=None, use_container_width=True)
```

点击下面的交互应用程序中的选项卡，可以查看启用和禁用Streamlit主题的图表。

<Cloud src="https://doc-plotly-chart-theme.streamlit.app/?embed=true" height="525" />

如果你想知道你自己的定制是否会被考虑进去，不用担心！你仍然可以对图表配置进行更改。换句话说，虽然我们现在默认启用了Streamlit主题，但你可以使用自定义的颜色或字体来覆盖它。例如，如果你想要将图表线条的颜色设置为绿色而不是默认的红色，你可以这样做！

下面是一个Plotly图表的示例，其中定义了一个自定义颜色比例尺并进行了反映：

```python
import plotly.express as px
import streamlit as st

st.subheader("Define a custom colorscale")
df = px.data.iris()
fig = px.scatter(
    df,
    x="sepal_width",
    y="sepal_length",
    color="sepal_length",
    color_continuous_scale="reds",
)

tab1, tab2 = st.tabs(["Streamlit theme (default)", "Plotly native theme"])
with tab1:
    st.plotly_chart(fig, theme="streamlit", use_container_width=True)
with tab2:
    st.plotly_chart(fig, theme=None, use_container_width=True)
```

注意，即使启用了Streamlit主题，自定义颜色比例仍然会反映在图表中👇

<Cloud src="https://doc-plotly-custom-colors.streamlit.app/?embed=true" height="650" />

要查看更多使用Streamlit主题和不使用主题的Plotly图表示例，请访问[plotly.streamlit.app](https://plotly.streamlit.app)。
