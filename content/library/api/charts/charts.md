---
slug: /library/api-reference/charts
title: Chart elements
---

# 图表元素

Streamlit支持多种不同的图表库，并且我们的目标是不断增加对更多图表库的支持。目前，在我们的工具库中最基本的库是[Matplotlib](https://matplotlib.org/)。然后还有一些交互式图表库，如[Vega Lite](https://vega.github.io/vega-lite/)（2D图表）和[deck.gl](https://github.com/uber/deck.gl)（地图和3D图表）。最后，我们还提供了一些对Streamlit来说是“原生”的图表类型，
像 `st.line_chart` 和 `st.area_chart` 这样的函数。

<TileContainer>
<RefCard href="/library/api-reference/charts/st.line_chart">
<Image pure alt="screenshot" src="/images/api/line_chart.jpg" />

#### 简单折线图

显示一个折线图。

```python
st.line_chart(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.area_chart">
<Image pure alt="screenshot" src="/images/api/area_chart.jpg" />

#### 简单面积图

显示一个面积图。

```python
st.area_chart(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.bar_chart">
<Image pure alt="screenshot" src="/images/api/bar_chart.jpg" />

#### 简单柱状图

显示柱状图。

```python
st.bar_chart(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.map">
<Image pure alt="screenshot" src="/images/api/map.jpg" />

#### 在地图上显示散点图

在地图上显示带有数据点的图表。

```python
st.map(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.pyplot">
<Image pure alt="screenshot" src="/images/api/pyplot.jpg" />

#### Matplotlib

显示一个matplotlib.pyplot的图像。

```python
st.pyplot(my_mpl_figure)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.altair_chart">
<Image pure alt="screenshot" src="/images/api/vega_lite_chart.jpg" />

#### Altair

使用Altair库显示图表。

```python
st.altair_chart(my_altair_chart)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.vega_lite_chart">
<Image pure alt="screenshot" src="/images/api/vega_lite_chart.jpg" />

#### Vega-Lite

使用Vega-Lite库显示图表。

```python
st.vega_lite_chart(my_vega_lite_chart)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.plotly_chart">
<Image pure alt="screenshot" src="/images/api/plotly_chart.jpg" />

#### Plotly

显示一个交互式的Plotly图表。

```python
st.plotly_chart(my_plotly_chart)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.bokeh_chart">
<Image pure alt="screenshot" src="/images/api/bokeh_chart.jpg" />

#### Bokeh

展示一个交互式的Bokeh图表。

```python
st.bokeh_chart(my_bokeh_chart)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.pydeck_chart">
<Image pure alt="screenshot" src="/images/api/pydeck_chart.jpg" />

#### PyDeck

使用PyDeck库显示图表。

```python
st.pydeck_chart(my_pydeck_chart)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.graphviz_chart">
<Image pure alt="screenshot" src="/images/api/graphviz_chart.jpg" />

#### GraphViz

使用dagre-d3库显示图形。

```python
st.graphviz_chart(my_graphviz_spec)
```

</RefCard>
</TileContainer>

<ComponentSlider>

<ComponentCard href="https://github.com/tvst/plost">

<Image pure alt="screenshot" src="/images/api/components/plost.jpg" />

#### Plost

一个Streamlit的看似简单的绘图库。由[@tvst](https://github.com/tvst)创建。

```python
import plost
plost.line_chart(my_dataframe, x='time', y='stock_value', color='stock_name',)
```

</ComponentCard>

<ComponentCard href="https://github.com/facebookresearch/hiplot">

<Image pure alt="screenshot" src="/images/api/components/hiplot.jpg" />

#### HiPlot

高维交互式绘图工具。由[@facebookresearch](https://github.com/facebookresearch)创建。

```python
data = [{'dropout':0.1, 'lr': 0.001, 'loss': 10.0, 'optimizer': 'SGD'}, {'dropout':0.15, 'lr': 0.01, 'loss': 3.5, 'optimizer': 'Adam'}, {'dropout':0.3, 'lr': 0.1, 'loss': 4.5, 'optimizer': 'Adam'}]
hip.Experiment.from_iterable(data).display()
```

</ComponentCard>

<ComponentCard href="https://github.com/andfanilo/streamlit-echarts">

<Image pure alt="screenshot" src="/images/api/components/echarts.jpg" />

#### ECharts

高维度交互绘图。由[@andfanilo](https://github.com/andfanilo)创建。

```python
from streamlit_echarts import st_echarts
st_echarts(options=options)
```

</ComponentCard>

<ComponentCard href="https://github.com/randyzwitch/streamlit-folium">

<Image pure alt="screenshot" src="/images/api/components/folium.jpg" />

#### Streamlit Folium

Streamlit组件，用于渲染Folium地图。由[@randyzwitch](https://github.com/randyzwitch)创建。

```python
m = folium.Map(location=[39.949610, -75.150282], zoom_start=16)
st_data = st_folium(m, width=725)
```

</ComponentCard>

<ComponentCard href="https://github.com/explosion/spacy-streamlit">

<Image pure alt="screenshot" src="/images/api/components/spacy.jpg" />

#### Spacy-Streamlit

spaCy构建块和可视化工具，适用于Streamlit应用程序。由[@explosion](https://github.com/explosion)创建。

```python
models = ["en_core_web_sm", "en_core_web_md"]
spacy_streamlit.visualize(models, "Sundar Pichai is the CEO of Google.")
```

</ComponentCard>

<ComponentCard href="https://github.com/ChrisDelClea/streamlit-agraph">

<Image pure alt="screenshot" src="/images/api/components/agraph.jpg" />

#### Streamlit Agraph

基于[react-grah-vis](https://github.com/crubier/react-graph-vis)的Streamlit图形可视化组件。由[@ChrisDelClea](https://github.com/ChrisDelClea)创建。

```python
from streamlit_agraph import agraph, Node, Edge, Config
agraph(nodes=nodes, edges=edges, config=config)
```

</ComponentCard>

<ComponentCard href="https://github.com/andfanilo/streamlit-lottie">

<Image pure alt="screenshot" src="/images/api/components/lottie.jpg" />

#### Streamlit Lottie

在您的Streamlit应用程序中集成[Lottie](https://lottiefiles.com/)动画。由[@andfanilo](https://github.com/andfanilo)创建。

```python
lottie_hello = load_lottieurl("https://assets5.lottiefiles.com/packages/lf20_V9t630.json")
st_lottie(lottie_hello, key="hello")
```

</ComponentCard>

<ComponentCard href="https://github.com/null-jones/streamlit-plotly-events">

<Image pure alt="screenshot" src="/images/api/components/plotly-events.jpg" />

#### Plotly 事件

使Plotly图表具有交互功能！由[@null-jones](https://github.com/null-jones/)创建。

```python
fig = px.line(x=[1], y=[1])
selected_points = plotly_events(fig)
```

</ComponentCard>

<ComponentCard href="https://extras.streamlit.app/">

<Image pure alt="screenshot" src="/images/api/components/extras-chart-annotations.jpg" />

#### Streamlit Extras

一个包含有用的Streamlit扩展的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
chart += get_annotations_chart(annotations=[("Mar 01, 2008", "Pretty good day for GOOG"), ("Dec 01, 2007", "Something's going wrong for GOOG & AAPL"), ("Nov 01, 2008", "Market starts again thanks to..."), ("Dec 01, 2009", "Small crash for GOOG after..."),],)
st.altair_chart(chart, use_container_width=True)
```

</ComponentCard>

</ComponentSlider>