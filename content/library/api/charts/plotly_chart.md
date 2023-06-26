---
description: st.plotly_chart displays an interactive Plotly chart.
slug: /library/api-reference/charts/st.plotly_chart
title: st.plotly_chart
---

<Autofunction function="streamlit.plotly_chart" />

### 主题

默认情况下，Plotly 图表使用 Streamlit 主题进行显示。该主题设计简洁、用户友好，并结合了 Streamlit 的颜色调色板。另一个好处是，您的图表能更好地与应用程序的设计整合。

Streamlit 主题从 Streamlit 1.16.0 版本开始可用，通过 `theme="streamlit"` 关键字参数进行设置。如果要禁用它，并使用 Plotly 的原生主题，请使用 `theme=None`。

让我们来看一个使用Streamlit主题和原生Plotly主题的图表示例：

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

点击下面的交互应用程序中的选项卡，查看启用和禁用Streamlit主题的图表。

<Cloud src="https://doc-plotly-chart-theme.streamlit.app/?embed=true" height="525" />

如果您想知道自定义的配置是否仍然会被考虑，请不用担心！您仍然可以对图表配置进行更改。换句话说，尽管我们现在默认启用Streamlit主题，但您可以使用自定义颜色或字体进行覆盖。例如，如果您想将图表线条改为绿色而不是默认的红色，您可以这样做！

以下是一个Plotly图表的示例，其中定义了一个自定义颜色比例尺并反映了出来：

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

请注意，即使启用了Streamlit主题，自定义颜色比例仍会在图表中反映出来👇

<Cloud src="https://doc-plotly-custom-colors.streamlit.app/?embed=true" height="650" />

要查看更多使用Streamlit主题和不使用主题的Plotly图表示例，请访问[plotly.streamlit.app](https://plotly.streamlit.app)。