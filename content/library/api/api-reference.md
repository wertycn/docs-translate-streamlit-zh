---
title: API 参考
slug: /library/api-reference
next: caching
previous: index.md
---

# API 参考

Streamlit 使您能够轻松可视化、变异和共享数据。API 参考按活动类型组织，例如显示数据或优化性能。每个部分都包括与活动类型相关联的方法，包括示例。

浏览下面的 API，并单击以了解有关任何可用命令的更多信息！ 🎈

## 几乎可以显示任何内容

<TileContainer>
<RefCard href="/library/api-reference/write-magic/st.write">

#### st.write

将参数写入应用程序。

```python
st.write("Hello **world**!")
st.write(my_data_frame)
st.write(my_mpl_figure)
```

</RefCard>
<RefCard href="/library/api-reference/write-magic/magic">

#### Magic

每当Streamlit在单独的一行上看到变量或字面值时，它会自动使用`st.write`将其写入您的应用程序中。

```python
"Hello **world**!"
my_data_frame
my_mpl_figure
```

</RefCard>
</TileContainer>

## 文本元素

<TileContainer>
<RefCard href="/library/api-reference/text/st.markdown">

<Image pure alt="screenshot" src="/images/api/markdown.jpg" />

#### Markdown

以Markdown格式显示字符串。

```python
st.markdown("Hello **world**!")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.title">

<Image pure alt="screenshot" src="/images/api/title.jpg" />

#### 标题

以标题格式显示文本。

```python
st.title("The app title")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.header">

<Image pure alt="screenshot" src="/images/api/header.jpg" />

#### 头部

以标题格式显示文本。

```python
st.header("This is a header")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.subheader">

<Image pure alt="screenshot" src="/images/api/subheader.jpg" />

#### 子标题

以子标题格式显示文本。

```python
st.subheader("This is a subheader")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.caption">

<Image pure alt="screenshot" src="/images/api/caption.jpg" />

#### 标题

以小字体显示文本。

```python
st.caption("This is written small caption text")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.code">

<Image pure alt="screenshot" src="/images/api/code.jpg" />

#### Code block

Display a code block with optional syntax highlighting.

```python
st.code("a = 1234")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.text">

<Image pure alt="screenshot" src="/images/api/text.jpg" />

#### Preformatted text

Write fixed-width and preformatted text.

```python
st.text("Hello world")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.latex">

<Image pure alt="screenshot" src="/images/api/latex.jpg" />

#### LaTeX

以LaTeX格式显示数学表达式。

```python
st.latex("\int a x^2 \,dx")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.divider">

<Image pure alt="screenshot" src="/images/api/divider.jpg" />

#### 分隔线

显示一条水平线。

```python
st.divider()
```

</RefCard>
</TileContainer>

<ComponentSlider>
<ComponentCard href="https://github.com/tvst/st-annotated-text">

<Image pure alt="screenshot" src="/images/api/components/annotated-text.jpg" />

#### 带注释的文本

在Streamlit应用程序中显示带注释的文本。由[@tvst](https://github.com/tvst)创建。

```python
annotated_text("This ", ("is", "verb"), " some ", ("annotated", "adj"), ("text", "noun"), " for those of ", ("you", "pronoun"), " who ", ("like", "verb"), " this sort of ", ("thing", "noun"), ".")
```

</ComponentCard>

<ComponentCard href="https://github.com/andfanilo/streamlit-drawable-canvas">

<Image pure alt="screenshot" src="/images/api/components/drawable-canvas.jpg" />

#### 可绘制画布

使用[Fabric.js](http://fabricjs.com/)提供一个绘图画布。由[@andfanilo](https://github.com/andfanilo)创建。

```python
st_canvas(fill_color="rgba(255, 165, 0, 0.3)", stroke_width=stroke_width, stroke_color=stroke_color, background_color=bg_color, background_image=Image.open(bg_image) if bg_image else None, update_streamlit=realtime_update, height=150, drawing_mode=drawing_mode, point_display_radius=point_display_radius if drawing_mode == 'point' else 0, key="canvas",)
```

</ComponentCard>

<ComponentCard href="https://github.com/gagan3012/streamlit-tags">

<Image pure alt="screenshot" src="/images/api/components/tags.jpg" />

#### 标签

为您的Streamlit应用添加标签。由[@gagan3012](https://github.com/gagan3012)创建。

```python
st_tags(label='# Enter Keywords:', text='Press enter to add more', value=['Zero', 'One', 'Two'], suggestions=['five', 'six', 'seven', 'eight', 'nine', 'three', 'eleven', 'ten', 'four'], maxtags = 4, key='1')
```

</ComponentCard>

<ComponentCard href="https://github.com/JohnSnowLabs/nlu">

<Image pure alt="screenshot" src="/images/api/components/nlu.jpg" />

#### NLU

在数据帧上应用文本挖掘。由[@JohnSnowLabs](https://github.com/JohnSnowLabs/)创建。

```python
nlu.load('sentiment').predict('I love NLU! <3')
```

</ComponentCard>

<ComponentCard href="https://extras.streamlit.app/">

<Image pure alt="screenshot" src="/images/api/components/extras-mentions.jpg" />

#### Streamlit Extras

一个包含有用的Streamlit扩展的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
mention(label="An awesome Streamlit App", icon="streamlit",  url="https://extras.streamlit.app",)
```

</ComponentCard>
</ComponentSlider>

## 数据元素

<TileContainer>
<RefCard href="/library/api-reference/data/st.dataframe">
<Image pure alt="screenshot" src="/images/api/dataframe.jpg" />

#### 数据框

将数据框显示为交互式表格。

```python
st.dataframe(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/data/st.data_editor">

<Image pure alt="screenshot" src="/images/api/data_editor.jpg" />

#### 数据编辑器

显示一个数据编辑器小部件。

```python
edited = st.data_editor(df, num_rows="dynamic")
```

</RefCard>
<RefCard href="/library/api-reference/data/st.column_config">

<Image pure alt="screenshot" src="/images/api/column_config.jpg" />

#### 列配置

配置数据帧和数据编辑器的显示和编辑行为。

```python
st.column_config.NumberColumn("Price (in USD)", min_value=0, format="$%d")
```

</RefCard>

<RefCard href="/library/api-reference/data/st.table">
<Image pure alt="screenshot" src="/images/api/table.jpg" />

#### 静态表格

显示一个静态表格。

```python
st.table(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/data/st.metric">
<Image pure alt="screenshot" src="/images/api/metric.jpg" />

#### 指标

以大胆的字体显示指标，可选择性地显示指标变化的指示器。

```python
st.metric("My metric", 42, 2)
```

</RefCard>
<RefCard href="/library/api-reference/data/st.json">
<Image pure alt="screenshot" src="/images/api/json.jpg" />

#### 字典和JSON

将对象或字符串以漂亮的JSON字符串形式显示。

```python
st.json(my_dict)
```

</RefCard>
</TileContainer>

<ComponentSlider>

<ComponentCard href="https://github.com/PablocFonseca/streamlit-aggrid">

<Image pure alt="screenshot" src="/images/api/components/aggrid.jpg" />

#### Streamlit Aggrid

Streamlit的Ag-Grid组件的实现。由[@PablocFonseca](https://github.com/PablocFonseca)创建。

```python
df = pd.DataFrame({'col1': [1, 2, 3], 'col2': [4, 5, 6]})
grid_return = AgGrid(df, editable=True)

new_df = grid_return['data']
```

</ComponentCard>

<ComponentCard href="https://github.com/randyzwitch/streamlit-folium">

<Image pure alt="screenshot" src="/images/api/components/folium.jpg" />

#### Streamlit Folium

Streamlit Folium是一个用于渲染Folium地图的Streamlit组件。由[@randyzwitch](https://github.com/randyzwitch)创建。

```python
m = folium.Map(location=[39.949610, -75.150282], zoom_start=16)
folium.Marker([39.949610, -75.150282], popup="Liberty Bell", tooltip="Liberty Bell").add_to(m)

st_data = st_folium(m, width=725)
```

</ComponentCard>

<ComponentCard href="https://github.com/okld/streamlit-pandas-profiling">

<Image pure alt="screenshot" src="/images/api/components/pandas-profiling.jpg" />

#### Pandas Profiling

Streamlit的Pandas Profiling组件。由[@okld](https://github.com/okld/)创建。

```python
df = pd.read_csv("https://storage.googleapis.com/tf-datasets/titanic/train.csv")
pr = df.profile_report()

st_profile_report(pr)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/streamlit-image-coordinates">

<Image pure alt="screenshot" src="/images/api/components/image-coordinates.jpg" />

#### 图像坐标

获取图像上点击位置的坐标。由[@blackary](https://github.com/blackary/)创建。

```python
from streamlit_image_coordinates import streamlit_image_coordinates
value = streamlit_image_coordinates("https://placekitten.com/200/300")

st.write(value)
```

</ComponentCard>

<ComponentCard href="https://github.com/null-jones/streamlit-plotly-events">

<Image pure alt="screenshot" src="/images/api/components/plotly-events.jpg" />

#### Plotly 事件

使 Plotly 图表具有交互性！由 [@null-jones](https://github.com/null-jones/) 创建。

```python
from streamlit_plotly_events import plotly_events
fig = px.line(x=[1], y=[1])

selected_points = plotly_events(fig)
```

</ComponentCard>

<ComponentCard href="https://extras.streamlit.app/">

<Image pure alt="screenshot" src="/images/api/components/extras-metric-cards.jpg" />

#### Streamlit Extras

一个带有有用的Streamlit附加组件的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
from streamlit_extras.metric_cards import style_metric_cards
col3.metric(label="No Change", value=5000, delta=0)

style_metric_cards()
```

</ComponentCard>

</ComponentSlider>

## 图表元素

<TileContainer>
<RefCard href="/library/api-reference/charts/st.line_chart">
<Image pure alt="screenshot" src="/images/api/line_chart.jpg" />

#### 简单折线图

显示一张折线图。

```python
st.line_chart(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.area_chart">
<Image pure alt="screenshot" src="/images/api/area_chart.jpg" />

#### 简单区域图表

显示一个区域图表。

```python
st.area_chart(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.bar_chart">
<Image pure alt="screenshot" src="/images/api/bar_chart.jpg" />

#### Simple bar charts

Display a bar chart.

```python
st.bar_chart(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.map">
<Image pure alt="screenshot" src="/images/api/map.jpg" />

#### 在地图上显示散点图

显示一个带有散点的地图。

```python
st.map(my_data_frame)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.pyplot">
<Image pure alt="screenshot" src="/images/api/pyplot.jpg" />

#### Matplotlib

显示一个 matplotlib.pyplot 图形。

```python
st.pyplot(my_mpl_figure)
```

</RefCard>
<RefCard href="/library/api-reference/charts/st.altair_chart">
<Image pure alt="截图" src="/images/api/vega_lite_chart.jpg" />

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

显示一个交互式的Bokeh图表。

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

一款对于Streamlit来说，看似简单的绘图库。由[@tvst](https://github.com/tvst)创建。

```python
import plost
plost.line_chart(my_dataframe, x='time', y='stock_value', color='stock_name',)
```

</ComponentCard>

<ComponentCard href="https://github.com/facebookresearch/hiplot">

<Image pure alt="screenshot" src="/images/api/components/hiplot.jpg" />

#### HiPlot

高维交互式绘图。由[@facebookresearch](https://github.com/facebookresearch)创建。

```python
data = [{'dropout':0.1, 'lr': 0.001, 'loss': 10.0, 'optimizer': 'SGD'}, {'dropout':0.15, 'lr': 0.01, 'loss': 3.5, 'optimizer': 'Adam'}, {'dropout':0.3, 'lr': 0.1, 'loss': 4.5, 'optimizer': 'Adam'}]
hip.Experiment.from_iterable(data).display()
```

</ComponentCard>

<ComponentCard href="https://github.com/andfanilo/streamlit-echarts">

<Image pure alt="screenshot" src="/images/api/components/echarts.jpg" />

#### ECharts

高维交互式绘图。由[@andfanilo](https://github.com/andfanilo)创建。

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

使 Plotly 图表变得交互！由 [@null-jones](https://github.com/null-jones/) 创建。

```python
fig = px.line(x=[1], y=[1])
selected_points = plotly_events(fig)
```

</ComponentCard>

<ComponentCard href="https://extras.streamlit.app/">

<Image pure alt="screenshot" src="/images/api/components/extras-chart-annotations.jpg" />

#### Streamlit Extras

Streamlit Extras是一个包含有用的Streamlit扩展的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
chart += get_annotations_chart(annotations=[("Mar 01, 2008", "Pretty good day for GOOG"), ("Dec 01, 2007", "Something's going wrong for GOOG & AAPL"), ("Nov 01, 2008", "Market starts again thanks to..."), ("Dec 01, 2009", "Small crash for GOOG after..."),],)
st.altair_chart(chart, use_container_width=True)
```

</ComponentCard>

</ComponentSlider>

## 输入小部件

<TileContainer>
<RefCard href="/library/api-reference/widgets/st.button">

<Image pure alt="screenshot" src="/images/api/button.jpg" />

#### 按钮

显示一个按钮小部件。

```python
clicked = st.button("Click me")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.experimental_data_editor">

<Image pure alt="screenshot" src="/images/api/data_editor.jpg" />

#### 数据编辑器

显示一个数据编辑器小部件。

```python
edited = st.experimental_data_editor(df, num_rows="dynamic")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.download_button">

<Image pure alt="screenshot" src="/images/api/download_button.jpg" />

#### 下载按钮

显示一个下载按钮小部件。

```python
st.download_button("Download file", file)
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.checkbox">

<Image pure alt="screenshot" src="/images/api/checkbox.jpg" />

#### 复选框

显示一个复选框小部件。

```python
selected = st.checkbox("I agree")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.radio">

<Image pure alt="screenshot" src="/images/api/radio.jpg" />

#### 单选按钮

显示一个单选按钮小部件。

```python
choice = st.radio("Pick one", ["cats", "dogs"])
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.selectbox">

<Image pure alt="screenshot" src="/images/api/selectbox.jpg" />

#### 选择框

显示一个选择小部件。

```python
choice = st.selectbox("Pick one", ["cats", "dogs"])
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.multiselect">

<Image pure alt="screenshot" src="/images/api/multiselect.jpg" />

#### 多选

显示一个多选小部件。多选小部件一开始是空的。

```python
choices = st.multiselect("Buy", ["milk", "apples", "potatoes"])
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.slider">

<Image pure alt="screenshot" src="/images/api/slider.jpg" />

#### 滑块

显示滑块小部件。

```python
number = st.slider("Pick a number", 0, 100)
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.select_slider">

<Image pure alt="screenshot" src="/images/api/select_slider.jpg" />

#### 选择滑块

显示一个滑块小部件，用于从列表中选择项目。

```python
size = st.select_slider("Pick a size", ["S", "M", "L"])
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.text_input">

<Image pure alt="screenshot" src="/images/api/text_input.jpg" />

#### 文本输入

显示一个单行文本输入小部件。

```python
name = st.text_input("First name")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.number_input">

<Image pure alt="screenshot" src="/images/api/number_input.jpg" />

#### 数字输入

显示一个数字输入小部件。

```python
choice = st.number_input("Pick a number", 0, 10)
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.text_area">

<Image pure alt="screenshot" src="/images/api/text_area.jpg" />

#### 文本区域

显示一个多行文本输入小部件。

```python
text = st.text_area("Text to translate")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.date_input">

<Image pure alt="screenshot" src="/images/api/date_input.jpg" />

#### 日期输入

显示一个日期输入小部件。

```python
date = st.date_input("Your birthday")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.time_input">

<Image pure alt="screenshot" src="/images/api/time_input.jpg" />

#### 时间输入

显示一个时间输入小部件。

```python
time = st.time_input("Meeting time")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.file_uploader">

<Image pure alt="screenshot" src="/images/api/file_uploader.jpg" />

#### 文件上传器

显示一个文件上传器小部件。

```python
data = st.file_uploader("Upload a CSV")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.camera_input">

<Image pure alt="screenshot" src="/images/api/camera_input.jpg" />

#### 相机输入

显示一个小部件，允许用户直接从相机上传图片。

```python
image = st.camera_input("Take a picture")
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.color_picker">

<Image pure alt="screenshot" src="/images/api/color_picker.jpg" />

#### 颜色选择器

显示一个颜色选择器小部件。

```python
color = st.color_picker("Pick a color")
```

</RefCard>
</TileContainer>

<ComponentSlider>

<ComponentCard href="https://github.com/okld/streamlit-elements">

<Image pure alt="screenshot" src="/images/api/components/elements.jpg" />

#### Streamlit Elements

在Streamlit中创建一个可拖动和可调整大小的仪表板。由[@okls](https://github.com/okls)创建。

```python
from streamlit_elements import elements, mui, html

with elements("new_element"):
  mui.Typography("Hello world")
```

</ComponentCard>

<ComponentCard href="https://github.com/gagan3012/streamlit-tags">

<Image pure alt="screenshot" src="/images/api/components/tags.jpg" />

#### 标签

在您的Streamlit应用程序中添加标签。由[@gagan3012](https://github.com/gagan3012)创建。

```python
from streamlit_tags import st_tags

st_tags(label='# Enter Keywords:', text='Press enter to add more', value=['Zero', 'One', 'Two'],
suggestions=['five', 'six', 'seven', 'eight', 'nine', 'three', 'eleven', 'ten', 'four'], maxtags = 4, key='1')
```

</ComponentCard>

<ComponentCard href="https://github.com/Wirg/stqdm">

<Image pure alt="screenshot" src="/images/api/components/stqdm.jpg" />

#### Stqdm

在Streamlit应用程序中处理进度条的最简单方法。由[@Wirg](https://github.com/Wirg)创建。

```python
from stqdm import stqdm

for _ in stqdm(range(50)):
    sleep(0.5)
```

</ComponentCard>

<ComponentCard href="https://github.com/innerdoc/streamlit-timeline">

<Image pure alt="screenshot" src="/images/api/components/timeline.jpg" />

#### 时间轴

使用[TimelineJS](https://timeline.knightlab.com/)在Streamlit应用中显示时间轴。由[@innerdoc](https://github.com/innerdoc)创建。

```python
from streamlit_timeline import timeline

with open('example.json', "r") as f:
  timeline(f.read(), height=800)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/streamlit-camera-input-live">

<Image pure alt="screenshot" src="/images/api/components/camera-live.jpg" />

#### 实时相机输入

用于替代st.camera_input的相机实时输入组件。由[@blackary](https://github.com/blackary)创建。

```python
from camera_input_live import camera_input_live

image = camera_input_live()
st.image(value)
```

</ComponentCard>

<ComponentCard href="https://github.com/okld/streamlit-ace">

<Image pure alt="screenshot" src="/images/api/components/ace.jpg" />

#### Streamlit Ace

Streamlit的Ace编辑器组件。由[@okld](https://github.com/okld)创建。

```python
from streamlit_ace import st_ace

content = st_ace()
content
```

</ComponentCard>

<ComponentCard href="https://github.com/AI-Yash/st-chat">

<Image pure alt="screenshot" src="/images/api/components/chat.jpg" />

#### Streamlit 聊天界面

Streamlit聊天界面组件。由[@AI-Yash](https://github.com/AI-Yash)创建。

```python
from streamlit_chat import message

message("My message")
message("Hello bot!", is_user=True)  # align's the message to the right
```

</ComponentCard>

<ComponentCard href="https://github.com/victoryhb/streamlit-option-menu">

<Image pure alt="screenshot" src="/images/api/components/option-menu.jpg" />

#### Streamlit选项菜单

从菜单中的选项列表中选择单个项目。由[@victoryhb](https://github.com/victoryhb)创建。

```python
from streamlit_option_menu import option_menu

option_menu("Main Menu", ["Home", 'Settings'],
  icons=['house', 'gear'], menu_icon="cast", default_index=1)
```

</ComponentCard>

<ComponentCard href="https://extras.streamlit.app/">

<Image pure alt="screenshot" src="/images/api/components/extras-toggle.jpg" />

#### Streamlit Extras

一个带有有用的Streamlit附加功能的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
from streamlit_extras.stoggle import stoggle

stoggle(
    "Click me!", """🥷 Surprise! Here's some additional content""",)
```

</ComponentCard>

</ComponentSlider>

## 媒体元素

<TileContainer>
<RefCard href="/library/api-reference/media/st.image">

<Image pure alt="screenshot" src="/images/api/image.jpg" />

#### 图像

显示一张或多张图片。

```python
st.image(numpy_array)
st.image(image_bytes)
st.image(file)
st.image("https://example.com/myimage.jpg")
```

</RefCard>
<RefCard href="/library/api-reference/media/st.audio">

<Image pure alt="screenshot" src="/images/api/audio.jpg" />

#### 音频

显示一个音频播放器。

```python
st.audio(numpy_array)
st.audio(audio_bytes)
st.audio(file)
st.audio("https://example.com/myaudio.mp3", format="audio/mp3")
```

</RefCard>
<RefCard href="/library/api-reference/media/st.video">

<Image pure alt="screenshot" src="/images/api/video.jpg" />

#### 视频

显示一个视频播放器。

```python
st.video(numpy_array)
st.video(video_bytes)
st.video(file)
st.video("https://example.com/myvideo.mp4", format="video/mp4")
```

</RefCard>
</TileContainer>

<ComponentSlider>

<ComponentCard href="https://github.com/whitphx/streamlit-webrtc">

<Image pure alt="screenshot" src="/images/api/components/webrtc.jpg" />

#### Streamlit Webrtc

使用Streamlit处理和传输实时的视频/音频流。由[@whitphx](https://github.com/whitphx)创建。

```python
from streamlit_webrtc import webrtc_streamer

webrtc_streamer(key="sample")
```

</ComponentCard>

<ComponentCard href="https://github.com/andfanilo/streamlit-drawable-canvas">

<Image pure alt="screenshot" src="/images/api/components/drawable-canvas.jpg" />

#### 可绘制画布

使用[Fabric.js](http://fabricjs.com/)提供一个绘图画布。由[@andfanilo](https://github.com/andfanilo)创建。

```python
from streamlit_drawable_canvas import st_canvas

st_canvas(fill_color="rgba(255, 165, 0, 0.3)", stroke_width=stroke_width, stroke_color=stroke_color, background_color=bg_color, background_image=Image.open(bg_image) if bg_image else None, update_streamlit=realtime_update, height=150, drawing_mode=drawing_mode, point_display_radius=point_display_radius if drawing_mode == 'point' else 0, key="canvas",)
```

</ComponentCard>

<ComponentCard href="https://github.com/fcakyon/streamlit-image-comparison">

<Image pure alt="screenshot" src="/images/api/components/image-comparison.jpg" />

#### 图片对比

使用[JuxtaposeJS](https://juxtapose.knightlab.com/)实现通过滑块对比图片功能。由[@fcakyon](https://github.com/fcakyon)创建。

```python
from streamlit_image_comparison import image_comparison

image_comparison(img1="image1.jpg", img2="image2.jpg",)
```

</ComponentCard>

<ComponentCard href="https://github.com/turner-anderson/streamlit-cropper">

<Image pure alt="screenshot" src="/images/api/components/cropper.jpg" />

#### Streamlit 图像裁剪器

一个简单的用于Streamlit的图像裁剪器。由[@turner-anderson](https://github.com/turner-anderson)创建。

```python
from streamlit_cropper import st_cropper

st_cropper(img, realtime_update=realtime_update, box_color=box_color, aspect_ratio=aspect_ratio)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/streamlit-image-coordinates">

<Image pure alt="screenshot" src="/images/api/components/image-coordinates.jpg" />

#### 图像坐标

获取图像上点击位置的坐标。由[@blackary](https://github.com/blackary/)创建。

```python
from streamlit_image_coordinates import streamlit_image_coordinates

streamlit_image_coordinates("https://placekitten.com/200/300")
```

</ComponentCard>

<ComponentCard href="https://github.com/andfanilo/streamlit-lottie">

<Image pure alt="screenshot" src="/images/api/components/lottie.jpg" />

#### Streamlit Lottie

在Streamlit应用程序中集成[Lottie](https://lottiefiles.com/)动画。由[@andfanilo](https://github.com/andfanilo)创建。

```python
lottie_hello = load_lottieurl("https://assets5.lottiefiles.com/packages/lf20_V9t630.json")

st_lottie(lottie_hello, key="hello")
```

</ComponentCard>

</ComponentSlider>

## 布局和容器

<TileContainer>
<RefCard href="/library/api-reference/layout/st.sidebar">

<Image pure alt="screenshot" src="/images/api/sidebar.jpg" />

#### 侧边栏

在侧边栏中显示项目。

```python
st.sidebar.write("This lives in the sidebar")
st.sidebar.button("Click me!")
```

</RefCard>
<RefCard href="/library/api-reference/layout/st.columns">

<Image pure alt="screenshot" src="/images/api/columns.jpg" />

#### 列布局

插入作为并排列出的容器。

```python
col1, col2 = st.columns(2)
col1.write("this is column 1")
col2.write("this is column 2")
```

</RefCard>
<RefCard href="/library/api-reference/layout/st.tabs">

<Image pure alt="screenshot" src="/images/api/tabs.jpg" />

#### 标签页

将容器分成多个标签页插入。

```python
tab1, tab2 = st.tabs(["Tab 1", "Tab2"])
tab1.write("this is tab 1")
tab2.write("this is tab 2")
```

</RefCard>
<RefCard href="/library/api-reference/layout/st.expander">

<Image pure alt="screenshot" src="/images/api/expander.jpg" />

#### 展开器（Expander）

插入一个可以展开/折叠的多元素容器。

```python
with st.expander("Open to see more"):
  st.write("This is more content")
```

</RefCard>
<RefCard href="/library/api-reference/layout/st.container">

<Image pure alt="screenshot" src="/images/api/container.jpg" />

#### 容器

插入一个多元素容器。

```python
c = st.container()
st.write("This will show last")
c.write("This will show first")
c.write("This will show second")
```

</RefCard>
<RefCard href="/library/api-reference/layout/st.empty">

<Image pure alt="screenshot" src="/images/api/empty.jpg" />

#### 空白

插入一个单元素容器。

```python
c = st.empty()
st.write("This will show last")
c.write("This will be replaced")
c.write("This will show first")
```

</RefCard>
</TileContainer>

<ComponentSlider>

<ComponentCard href="https://github.com/okld/streamlit-elements">

<Image pure alt="screenshot" src="/images/api/components/elements.jpg" />

#### Streamlit Elements

在Streamlit中创建可拖动和可调整大小的仪表板。由[@okls](https://github.com/okls)创建。

```python
from streamlit_elements import elements, mui, html

with elements("new_element"):
  mui.Typography("Hello world")
```

</ComponentCard>

<ComponentCard href="https://github.com/lukasmasuch/streamlit-pydantic">

<Image pure alt="screenshot" src="/images/api/components/pydantic.jpg" />

#### Pydantic

从Pydantic模型和数据类自动生成Streamlit用户界面。由[@lukasmasuch](https://github.com/lukasmasuch)创建。

```python
import streamlit_pydantic as sp

sp.pydantic_form(key="my_form",
  model=ExampleModel)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/st_pages">

<Image pure alt="screenshot" src="/images/api/components/pages.jpg" />

#### Streamlit 页面

Streamlit多页面应用程序的实验版本。由[@blackary](https://github.com/blackary)创建。

```python
from st_pages import Page, show_pages, add_page_title

show_pages([ Page("streamlit_app.py", "Home", "🏠"),
  Page("other_pages/page2.py", "Page 2", ":books:"), ])
```

</ComponentCard>

</ComponentSlider>

## Display progress and status

<TileContainer>
<RefCard href="/library/api-reference/status/st.progress">

<Image pure alt="screenshot" src="/images/api/progress.jpg" />

#### Progress bar

Display a progress bar.

```python
for i in range(101):
  st.progress(i)
  do_something_slow()
```

</RefCard>
<RefCard href="/library/api-reference/status/st.spinner">

<Image pure alt="screenshot" src="/images/api/spinner.jpg" />

#### Spinner

在执行一段代码块时，临时显示一条消息。

```python
with st.spinner("Please wait..."):
  do_something_slow()
```

</RefCard>
<RefCard href="/library/api-reference/status/st.balloons">

<Image pure alt="screenshot" src="/images/api/balloons.jpg" />

#### 气球

显示庆祝气球！

```python
do_something()

# Celebrate when all done!
st.balloons()
```

</RefCard>
<RefCard href="/library/api-reference/status/st.snow">

<Image pure alt="screenshot" src="/images/api/snow.jpg" />

#### 雪花

显示庆祝的雪花！

```python
do_something()

# Celebrate when all done!
st.snow()
```

</RefCard>
<RefCard href="/library/api-reference/status/st.error">

<Image pure alt="screenshot" src="/images/api/error.jpg" />

#### 错误框

显示错误消息。

```python
st.error("We encountered an error")
```

</RefCard>
<RefCard href="/library/api-reference/status/st.warning">

<Image pure alt="screenshot" src="/images/api/warning.jpg" />

#### 警告框

显示警告信息。

```python
st.warning("Unable to fetch image. Skipping...")
```

</RefCard>
<RefCard href="/library/api-reference/status/st.info">

<Image pure alt="screenshot" src="/images/api/info.jpg" />

#### 信息框

显示一条信息提示。

```python
st.info("Dataset is updated every day at midnight.")
```

</RefCard>
<RefCard href="/library/api-reference/status/st.success">

<Image pure alt="screenshot" src="/images/api/success.jpg" />

#### 成功框

显示一个成功的消息。

```python
st.success("Match found!")
```

</RefCard>
<RefCard href="/library/api-reference/status/st.exception">

<Image pure alt="screenshot" src="/images/api/exception.jpg" />

#### 异常输出

显示异常信息。

```python
e = RuntimeError("This is an exception of type RuntimeError")
st.exception(e)
```

</RefCard>

</TileContainer>

<ComponentSlider>

<ComponentCard href="https://github.com/Wirg/stqdm">

<Image pure alt="screenshot" src="/images/api/components/stqdm.jpg" />

#### Stqdm

在Streamlit应用程序中处理进度条的最简单方法。由[@Wirg](https://github.com/Wirg)创建。

```python
from stqdm import stqdm

for _ in stqdm(range(50)):
    sleep(0.5)
```

</ComponentCard>

<ComponentCard href="https://github.com/Socvest/streamlit-custom-notification-box">

<Image pure alt="screenshot" src="/images/api/components/custom-notification-box.jpg" />

#### 自定义通知框

一个具有关闭功能的自定义通知框。由[@Socvest](https://github.com/Socvest)创建。

```python
from streamlit_custom_notification_box import custom_notification_box

styles = {'material-icons':{'color': 'red'}, 'text-icon-link-close-container': {'box-shadow': '#3896de 0px 4px'}, 'notification-text': {'':''}, 'close-button':{'':''}, 'link':{'':''}}
custom_notification_box(icon='info', textDisplay='We are almost done with your registration...', externalLink='more info', url='#', styles=styles, key="foo")
```

</ComponentCard>

<ComponentCard href="https://extras.streamlit.app/">

<Image pure alt="screenshot" src="/images/api/components/extras-emojis.jpg" />

#### Streamlit Extras

一个包含有用的Streamlit扩展的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
from streamlit_extras.let_it_rain import rain

rain(emoji="🎈", font_size=54,
  falling_speed=5, animation_length="infinite",)
```

</ComponentCard>

</ComponentSlider>

## 控制流程

<TileContainer>
<RefCard href="/library/api-reference/control-flow/st.form">

<!--<Image pure alt="screenshot" src="/images/api/form.jpg" />-->

#### 表单

创建一个表单，将元素批量组合在一起，并包含一个“提交”按钮。

```python
with st.form(key='my_form'):
    username = st.text_input("Username")
    password = st.text_input("Password")
    st.form_submit_button("Login")
```

</RefCard>
<RefCard href="/library/api-reference/control-flow/st.stop">

#### 停止执行

立即停止执行。

```python
st.stop()
```

</RefCard>
<RefCard href="/library/api-reference/control-flow/st.experimental_rerun">

#### 重新运行脚本

立即重新运行脚本。

```python
st.experimental_rerun()
```

\</RefCard>
\</TileContainer>

\ComponentSlider>

\ComponentCard href="https://github.com/kmcgrady/streamlit-autorefresh">

\Image pure alt="screenshot" src="/images/api/components/autorefresh.jpg" />

#### 自动刷新

在不绑定脚本的情况下强制刷新。由[@kmcgrady](https://github.com/kmcgrady)创建。

```python
from streamlit_autorefresh import st_autorefresh

st_autorefresh(interval=2000, limit=100,
  key="fizzbuzzcounter")
```

</ComponentCard>

<ComponentCard href="https://github.com/lukasmasuch/streamlit-pydantic">

<Image pure alt="screenshot" src="/images/api/components/pydantic.jpg" />

#### Pydantic

使用Pydantic模型和数据类自动生成Streamlit UI界面。由[@lukasmasuch](https://github.com/lukasmasuch)创建。

```python
import streamlit_pydantic as sp

sp.pydantic_form(key="my_form",
  model=ExampleModel)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/st_pages">

<Image pure alt="screenshot" src="/images/api/components/pages.jpg" />

#### Streamlit Pages

Streamlit多页面应用的实验性版本。由[@blackary](https://github.com/blackary)创建。

```python
from st_pages import Page, show_pages, add_page_title

show_pages([ Page("streamlit_app.py", "Home", "🏠"),
  Page("other_pages/page2.py", "Page 2", ":books:"), ])
```

</ComponentCard>

</ComponentSlider>

## 开发者工具

<ComponentSlider>

<ComponentCard href="https://github.com/okld/streamlit-pandas-profiling">

<Image pure alt="screenshot" src="/images/api/components/pandas-profiling.jpg" />

#### Pandas Profiling

Pandas Profiling是Streamlit的一个组件。由[@okld](https://github.com/okld/)创建。

```python
df = pd.read_csv("https://storage.googleapis.com/tf-datasets/titanic/train.csv")
pr = df.profile_report()

st_profile_report(pr)
```

</ComponentCard>

<ComponentCard href="https://github.com/okld/streamlit-ace">

<Image pure alt="screenshot" src="/images/api/components/ace.jpg" />

#### Streamlit Ace

Streamlit的Ace编辑器组件。由[@okld](https://github.com/okld)创建。

```python
from streamlit_ace import st_ace

content = st_ace()
content
```

</ComponentCard>

<ComponentCard href="https://github.com/jrieke/streamlit-analytics">

<Image pure alt="screenshot" src="/images/api/components/analytics.jpg" />

#### Streamlit分析

跟踪并可视化用户与您的Streamlit应用程序的交互。由[@jrieke](https://github.com/jrieke)创建。

```python
import streamlit_analytics

with streamlit_analytics.track():
    st.text_input("Write something")
```

</ComponentCard>

</ComponentSlider>

## 实用工具

<TileContainer>
<RefCard href="/library/api-reference/utilities/st.set_page_config">

#### 设置页面标题、网站图标等

配置页面的默认设置。

```python
st.set_page_config(
  page_title="My app",
  page_icon=":shark:",
)
```

</RefCard>
<RefCard href="/library/api-reference/utilities/st.echo">

<!--<Image pure alt="screenshot" src="/images/api/echo.jpg" />-->

#### Echo

在应用程序上显示一些代码，然后执行它。对于教程非常有用。

```python
with st.echo():
  st.write('This code will be printed')
```

</RefCard>
<RefCard href="/library/api-reference/utilities/st.help">

#### 获取帮助

显示对象的文档字符串，并进行格式化显示。

```python
st.help(st.write)
st.help(pd.DataFrame)
```

</RefCard>
<RefCard href="/library/api-reference/utilities/st.experimental_get_query_params">

#### 获取查询参数

返回当前显示在浏览器URL栏中的查询参数。

```python
st.experimental_get_query_params()
```

</RefCard>
<RefCard href="/library/api-reference/utilities/st.experimental_set_query_params">

#### 设置查询参数

设置在浏览器URL栏中显示的查询参数。

```python
st.experimental_set_query_params(
  show_map=True,
  selected=["asia"]
)
```

</RefCard>
</TileContainer>

## 修改图表

<TileContainer>
<RefCard href="/library/api-reference/mutate">

#### 添加行

将数据框附加到当前数据框的底部的特定位置，以进行优化的数据更新。

```python
element = st.line_chart(df)
element.add_rows(df_with_extra_rows)
```

</RefCard>
</TileContainer>

## 状态管理

<TileContainer>
<RefCard href="/library/api-reference/session-state">

#### 会话状态

会话状态是一种在每个用户会话中共享变量的方式。

```python
st.session_state['key'] = value
```

</RefCard>
</TileContainer>

## 连接和数据库

<ComponentSlider>

<ComponentCard href="https://github.com/mkhorasani/Streamlit-Authenticator">

<Image pure alt="screenshot" src="/images/api/components/authenticator.jpg" />

#### 鉴权模块

一个安全的鉴权模块，用于验证用户凭据。由[@mkhorasani](https://github.com/mkhorasani)创建。

```python
import streamlit_authenticator as stauth

authenticator = stauth.Authenticate( config['credentials'], config['cookie']['name'],
config['cookie']['key'], config['cookie']['expiry_days'], config['preauthorized'])
```

</ComponentCard>

<ComponentCard href="https://github.com/gagangoku/streamlit-ws-localstorage">

<Image pure alt="screenshot" src="/images/api/components/localstorage.jpg" />

#### WS localStorage

一个简单的同步方式，用于从您的应用程序中访问localStorage。由[@gagangoku](https://github.com/gagangoku)创建。

```python
from streamlit_ws_localstorage import injectWebsocketCode

ret = conn.setLocalStorageVal(key='k1', val='v1')
st.write('ret: ' + ret)
```

</ComponentCard>

<ComponentCard href="https://github.com/conradbez/streamlit-auth0">

<Image pure alt="截图" src="/images/api/components/auth0.jpg" />

#### Streamlit Auth0

在Streamlit中提供全面登录的最快方式。由[@conradbez](https://github.com/conradbez)创建。

```python
from auth0_component import login_button

user_info = login_button(clientId, domain = domain)
st.write(user_info)
```

</ComponentCard>

</ComponentSlider>

## 性能

<TileContainer>
<RefCard href="/library/api-reference/performance/st.cache_data" size="half">

#### 缓存数据

用于缓存返回数据的函数装饰器（例如数据帧转换、数据库查询、机器学习推断）。

```python
@st.cache_data
def long_function(param1, param2):
  # Perform expensive computation here or
  # fetch data from the web here
  return data
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.cache_resource" size="half">

#### 缓存资源

用于缓存返回全局资源的函数装饰器（例如数据库连接、ML模型）。

```python
@st.cache_resource
def init_model():
  # Return a global resource here
  return pipeline(
    "sentiment-analysis",
    model="distilbert-base-uncased-finetuned-sst-2-english"
  )
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.cache_data.clear" size="half">

#### 清除缓存数据

清除所有内存和磁盘数据缓存。

```python
@st.cache_data
def long_function(param1, param2):
  # Perform expensive computation here or
  # fetch data from the web here
  return data

if st.checkbox("Clear All"):
  # Clear values from *all* cache_data functions
  st.cache_data.clear()
```

</RefCard>

<RefCard href="/library/api-reference/performance/st.cache_resource.clear" size="half">

#### 清除缓存资源

清除所有`st.cache_resource`的缓存。

```python
@st.cache_resource
def init_model():
  # Return a global resource here
  return pipeline(
    "sentiment-analysis",
    model="distilbert-base-uncased-finetuned-sst-2-english"
  )

if st.checkbox("Clear All"):
  # Clear values from *all* cache_resource functions
  st.cache_data.clear()
```

## 连接和数据库

### 设置您的连接

<TileContainer>
<RefCard href="/library/api-reference/connections/st.experimental_connection" size="half">

<Image pure alt="screenshot" src="/images/api/connection.jpg" />

#### 创建连接

连接到数据源或API

```python
conn = st.experimental_connection('pets_db', type='sql')
pet_owners = conn.query('select * from pet_owners')
st.dataframe(pet_owners)
```

</RefCard>
</TileContainer>

### 内置连接

<TileContainer>

<RefCard href="/library/api-reference/connections/st.connections.sqlconnection" size="half">

<Image pure alt="screenshot" src="/images/api/connections.SQLConnection.jpg" />

#### SQLConnection

使用SQLAlchemy连接到SQL数据库的连接。

```python
conn = st.experimental_connection('sql')
```

</RefCard>

<RefCard href="/library/api-reference/connections/st.connections.snowparkconnection" size="half">

<Image pure alt="screenshot" src="/images/api/connections.SnowparkConnection.jpg" />

#### SnowparkConnection

与Snowflake Snowpark的连接。

```python
conn = st.experimental_connection('snowpark')
```

</RefCard>
</TileContainer>

### 第三方连接

<TileContainer>
<RefCard href="/library/api-reference/connections/st.connections.experimentalbaseconnection" size="half">

#### 连接基类

使用`ExperimentalBaseConnection`构建您自己的连接。

```python
class MyConnection(ExperimentalBaseConnection[myconn.MyConnection]):
    def _connect(self, **kwargs) -> MyConnection:
        return myconn.connect(**self._secrets, **kwargs)
    def query(self, query):
        return self._instance.query(query)
```

</RefCard>

</TileContainer>

## 个性化

<TileContainer>
<RefCard href="/library/api-reference/personalization/st.experimental_user" size="half">

#### 用户信息

`st.experimental_user` 返回有关 Streamlit Community Cloud 上私有应用程序的已登录用户的信息。

```python
if st.experimental_user.email == "foo@corp.com":
  st.write("Welcome back, ", st.experimental_user.email)
else:
  st.write("You are not authorized to view this page.")
```

</RefCard>
</TileContainer>
