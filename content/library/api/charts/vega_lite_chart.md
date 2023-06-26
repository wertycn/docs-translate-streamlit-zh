---
description: st.vega_lite_chart displays a chart using the Vega-Lite library.
slug: /library/api-reference/charts/st.vega_lite_chart
title: st.vega_lite_chart
---

<Autofunction function="streamlit.vega_lite_chart" />

### 主题设置

默认情况下，Vega-Lite图表使用Streamlit主题进行显示。该主题设计时时尚、用户友好，并集成了Streamlit的颜色调色板。另一个好处是，您的图表能更好地与应用程序的其他设计相融合。

从Streamlit 1.16.0开始，您可以通过`theme="streamlit"`关键字参数来使用Streamlit主题。如果要禁用主题，并使用Vega-Lite的原生主题，请使用`theme=None`。

让我们来看一个使用Streamlit主题和原生Vega-Lite主题的图表示例：

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

点击下面的交互式应用程序中的选项卡，可以查看启用和禁用Streamlit主题的图表。

<Cloud src="https://doc-vega-lite-theme.streamlit.app/?embed=true" height="500" />

如果你想知道你自己的自定义配置是否仍然会被考虑到，不用担心！你仍然可以对图表配置进行更改。换句话说，虽然我们现在默认启用Streamlit主题，但你可以用自定义的颜色或字体来覆盖它。例如，如果你想让图表线条变成绿色而不是默认的红色，你可以这样做！