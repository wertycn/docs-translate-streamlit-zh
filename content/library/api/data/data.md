---
标题：数据元素
网址：/library/api-reference/data
---

# 数据元素

在处理数据时，快速、交互式地以多个不同的角度可视化数据非常有价值。这正是Streamlit构建和优化的目的。

您可以通过[图表](#显示图表)来显示数据，也可以以原始形式显示数据。以下是您可以使用的Streamlit命令来显示和与原始数据进行交互。

<TileContainer>
<RefCard href="/library/api-reference/data/st.dataframe">
<Image pure alt="screenshot" src="/images/api/dataframe.jpg" />

#### 数据框

将数据框以交互式表格的形式展示。

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

配置数据框和数据编辑器的显示和编辑行为。

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

以大胆字体显示指标，可选择性地显示指标变化的指示器。

```python
st.metric("My metric", 42, 2)
```

</RefCard>
<RefCard href="/library/api-reference/data/st.json">
<Image pure alt="screenshot" src="/images/api/json.jpg" />

#### 字典和JSON

将对象或字符串以漂亮的格式打印为JSON字符串。

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

Streamlit组件用于呈现Folium地图。由[@randyzwitch](https://github.com/randyzwitch)创建。

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

一个包含有用的Streamlit扩展的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
from streamlit_extras.metric_cards import style_metric_cards
col3.metric(label="No Change", value=5000, delta=0)

style_metric_cards()
```

</ComponentCard>

</ComponentSlider>
