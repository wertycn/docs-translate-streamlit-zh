---
title: st.vega_lite_chart
slug: /library/api-reference/charts/st.vega_lite_chart
description: st.vega_lite_chart使用Vega-Lite库显示图表。

<Autofunction function="streamlit.vega_lite_chart" />

### 主题

默认情况下，Vega-Lite图表使用Streamlit主题显示。该主题简洁、用户友好，并融入了Streamlit的颜色调色板。优势是您的图表能更好地与应用程序的其他设计融合在一起。

Streamlit主题可在Streamlit 1.16.0及更高版本中通过`theme="streamlit"`关键参数进行使用。要禁用它并使用Vega-Lite的原生主题，请改用`theme=None`。

让我们来看一个使用Streamlit主题和原生Vega-Lite主题的图表示例:

```python
import streamlit as st
from vega_datasets import data

source = data.cars()

chart = {
    "mark": "point",
    "encoding": {
        "x": {
            "field": "Horsepower",
            "type": "quantitative",
        },
        "y": {
            "field": "Miles_per_Gallon",
            "type": "quantitative",
        },
        "color": {"field": "Origin", "type": "nominal"},
        "shape": {"field": "Origin", "type": "nominal"},
    },
}

tab1, tab2 = st.tabs(["Streamlit theme (default)", "Vega-Lite native theme"])

with tab1:
    # Use the Streamlit theme.
    # This is the default. So you can also omit the theme argument.
    st.vega_lite_chart(
        source, chart, theme="streamlit", use_container_width=True
    )
with tab2:
    st.vega_lite_chart(
        source, chart, theme=None, use_container_width=True
    )
```

点击下面交互式应用程序中的选项卡，可以查看启用和禁用Streamlit主题的图表。

<Cloud src="https://doc-vega-lite-theme.streamlit.app/?embed=true" height="500" />

如果您想知道自己的自定义配置是否仍然会被考虑到，不用担心！您仍然可以对图表配置进行更改。换句话说，尽管我们现在默认启用Streamlit主题，但您可以使用自定义颜色或字体覆盖它。例如，如果您希望图表线条是绿色而不是默认的红色，您可以这样做！
