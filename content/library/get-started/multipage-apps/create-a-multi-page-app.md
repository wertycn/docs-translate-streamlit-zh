---
slug: /library/get-started/multipage-apps/create-a-multipage-app
title: Create a multipage app
---

# 创建一个多页面应用

在[上一节](/library/get-started/multipage-apps)中，我们学习了创建多页面应用所需的内容，包括如何定义页面、构建和运行多页面应用，以及在用户界面中导航页面。如果您需要恢复记忆，现在是一个好时机去看一下。

在本指南中，让我们利用对多页面应用的理解，将熟悉的`streamlit hello`命令转换为一个多页面应用！

## 动机

在 Streamlit 1.10.0 之前，streamlit hello 命令是一个庞大的单页面应用程序。由于不支持多页面，我们使用侧边栏中的 `st.selectbox` 来选择要运行的内容来拆分应用程序的内容。内容由三个演示用于绘图、地图和数据框组成。

下面是代码和单页面应用程序的示例：

<details>
<summary><b><code>hello.py</code></b>  (👈 点击展开)</summary>
<br />

```python
import streamlit as st

def intro():
    import streamlit as st

    st.write("# Welcome to Streamlit! 👋")
    st.sidebar.success("Select a demo above.")

    st.markdown(
        """
        Streamlit is an open-source app framework built specifically for
        Machine Learning and Data Science projects.

        **👈 Select a demo from the dropdown on the left** to see some examples
        of what Streamlit can do!

        ### Want to learn more?

        - Check out [streamlit.io](https://streamlit.io)
        - Jump into our [documentation](https://docs.streamlit.io)
        - Ask a question in our [community
          forums](https://discuss.streamlit.io)

        ### See more complex demos

        - Use a neural net to [analyze the Udacity Self-driving Car Image
          Dataset](https://github.com/streamlit/demo-self-driving)
        - Explore a [New York City rideshare dataset](https://github.com/streamlit/demo-uber-nyc-pickups)
    """
    )

def mapping_demo():
    import streamlit as st
    import pandas as pd
    import pydeck as pdk

    from urllib.error import URLError

    st.markdown(f"# {list(page_names_to_funcs.keys())[2]}")
    st.write(
        """
        This demo shows how to use
[`st.pydeck_chart`](https://docs.streamlit.io/library/api-reference/charts/st.pydeck_chart)
to display geospatial data.
"""
    )

    @st.cache_data
    def from_data_file(filename):
        url = (
            "http://raw.githubusercontent.com/streamlit/"
            "example-data/master/hello/v1/%s" % filename
        )
        return pd.read_json(url)

    try:
        ALL_LAYERS = {
            "Bike Rentals": pdk.Layer(
                "HexagonLayer",
                data=from_data_file("bike_rental_stats.json"),
                get_position=["lon", "lat"],
                radius=200,
                elevation_scale=4,
                elevation_range=[0, 1000],
                extruded=True,
            ),
            "Bart Stop Exits": pdk.Layer(
                "ScatterplotLayer",
                data=from_data_file("bart_stop_stats.json"),
                get_position=["lon", "lat"],
                get_color=[200, 30, 0, 160],
                get_radius="[exits]",
                radius_scale=0.05,
            ),
            "Bart Stop Names": pdk.Layer(
                "TextLayer",
                data=from_data_file("bart_stop_stats.json"),
                get_position=["lon", "lat"],
                get_text="name",
                get_color=[0, 0, 0, 200],
                get_size=15,
                get_alignment_baseline="'bottom'",
            ),
            "Outbound Flow": pdk.Layer(
                "ArcLayer",
                data=from_data_file("bart_path_stats.json"),
                get_source_position=["lon", "lat"],
                get_target_position=["lon2", "lat2"],
                get_source_color=[200, 30, 0, 160],
                get_target_color=[200, 30, 0, 160],
                auto_highlight=True,
                width_scale=0.0001,
                get_width="outbound",
                width_min_pixels=3,
                width_max_pixels=30,
            ),
        }
        st.sidebar.markdown("### Map Layers")
        selected_layers = [
            layer
            for layer_name, layer in ALL_LAYERS.items()
            if st.sidebar.checkbox(layer_name, True)
        ]
        if selected_layers:
            st.pydeck_chart(
                pdk.Deck(
                    map_style="mapbox://styles/mapbox/light-v9",
                    initial_view_state={
                        "latitude": 37.76,
                        "longitude": -122.4,
                        "zoom": 11,
                        "pitch": 50,
                    },
                    layers=selected_layers,
                )
            )
        else:
            st.error("Please choose at least one layer above.")
    except URLError as e:
        st.error(
            """
            **This demo requires internet access.**

            Connection error: %s
        """
            % e.reason
        )

def plotting_demo():
    import streamlit as st
    import time
    import numpy as np

    st.markdown(f'# {list(page_names_to_funcs.keys())[1]}')
    st.write(
        """
        This demo illustrates a combination of plotting and animation with
Streamlit. We're generating a bunch of random numbers in a loop for around
5 seconds. Enjoy!
"""
    )

    progress_bar = st.sidebar.progress(0)
    status_text = st.sidebar.empty()
    last_rows = np.random.randn(1, 1)
    chart = st.line_chart(last_rows)

    for i in range(1, 101):
        new_rows = last_rows[-1, :] + np.random.randn(5, 1).cumsum(axis=0)
        status_text.text("%i%% Complete" % i)
        chart.add_rows(new_rows)
        progress_bar.progress(i)
        last_rows = new_rows
        time.sleep(0.05)

    progress_bar.empty()

    # Streamlit widgets automatically run the script from top to bottom. Since
    # this button is not connected to any other logic, it just causes a plain
    # rerun.
    st.button("Re-run")


def data_frame_demo():
    import streamlit as st
    import pandas as pd
    import altair as alt

    from urllib.error import URLError

    st.markdown(f"# {list(page_names_to_funcs.keys())[3]}")
    st.write(
        """
        This demo shows how to use `st.write` to visualize Pandas DataFrames.

(Data courtesy of the [UN Data Explorer](http://data.un.org/Explorer.aspx).)
"""
    )

    @st.cache_data
    def get_UN_data():
        AWS_BUCKET_URL = "http://streamlit-demo-data.s3-us-west-2.amazonaws.com"
        df = pd.read_csv(AWS_BUCKET_URL + "/agri.csv.gz")
        return df.set_index("Region")

    try:
        df = get_UN_data()
        countries = st.multiselect(
            "Choose countries", list(df.index), ["China", "United States of America"]
        )
        if not countries:
            st.error("Please select at least one country.")
        else:
            data = df.loc[countries]
            data /= 1000000.0
            st.write("### Gross Agricultural Production ($B)", data.sort_index())

            data = data.T.reset_index()
            data = pd.melt(data, id_vars=["index"]).rename(
                columns={"index": "year", "value": "Gross Agricultural Product ($B)"}
            )
            chart = (
                alt.Chart(data)
                .mark_area(opacity=0.3)
                .encode(
                    x="year:T",
                    y=alt.Y("Gross Agricultural Product ($B):Q", stack=None),
                    color="Region:N",
                )
            )
            st.altair_chart(chart, use_container_width=True)
    except URLError as e:
        st.error(
            """
            **This demo requires internet access.**

            Connection error: %s
        """
            % e.reason
        )

page_names_to_funcs = {
    "—": intro,
    "Plotting Demo": plotting_demo,
    "Mapping Demo": mapping_demo,
    "DataFrame Demo": data_frame_demo
}

demo_name = st.sidebar.selectbox("Choose a demo", page_names_to_funcs.keys())
page_names_to_funcs[demo_name]()
```

</details>

<Cloud src="https://doc-hello.streamlit.app/?embed=true" height="700" />

注意文件的大小！每个应用程序“页面”都被编写为一个函数，并且使用selectbox来选择要显示的页面。随着我们的应用程序的增长，维护代码需要很多额外的开销。此外，我们受限于`st.selectbox`界面来选择要运行的“页面”，我们无法使用`st.set_page_config`自定义单个页面的标题，并且我们无法使用URL在页面之间导航。

## 将现有应用程序转换为多页面应用程序

既然我们已经确定了单页面应用程序的局限性，我们该怎么办呢？当然可以根据我们在前一节中的知识，将现有的应用程序转换为多页面应用程序！在高层次上，我们需要执行以下步骤：

1. 在“入口文件”(`hello.py`)所在的同一文件夹中创建一个新的`pages`文件夹
2. 将我们的入口文件重命名为`Hello.py`，以使侧边栏的标题大写
3. 在`pages`文件夹中创建三个新文件：
   - `pages/1_📈_Plotting_Demo.py`
   - `pages/2_🌍_Mapping_Demo.py`
   - `pages/3_📊_DataFrame_Demo.py`
4. 将`plotting_demo`、`mapping_demo`和`data_frame_demo`函数的内容移到第3步中对应的新文件中
5. 运行`streamlit run Hello.py`来查看您新转换的多页面应用程序！

现在，让我们逐步介绍整个过程，并查看代码中的相应变化。

## 创建入口文件

<details open>
<summary><code>Hello.py</code></summary>

```python
import streamlit as st

st.set_page_config(
    page_title="Hello",
    page_icon="👋",
)

st.write("# Welcome to Streamlit! 👋")

st.sidebar.success("Select a demo above.")

st.markdown(
    """
    Streamlit is an open-source app framework built specifically for
    Machine Learning and Data Science projects.
    **👈 Select a demo from the sidebar** to see some examples
    of what Streamlit can do!
    ### Want to learn more?
    - Check out [streamlit.io](https://streamlit.io)
    - Jump into our [documentation](https://docs.streamlit.io)
    - Ask a question in our [community
        forums](https://discuss.streamlit.io)
    ### See more complex demos
    - Use a neural net to [analyze the Udacity Self-driving Car Image
        Dataset](https://github.com/streamlit/demo-self-driving)
    - Explore a [New York City rideshare dataset](https://github.com/streamlit/demo-uber-nyc-pickups)
"""
)
```

</details>
<br />

我们将入口文件重命名为`Hello.py`，这样侧边栏的标题就会大写，并且只包含介绍页面的代码。此外，我们还可以使用`st.set_page_config`来自定义页面标题和网页图标，它会显示在浏览器选项卡中。我们也可以为每个页面进行相同的设置！

<Image src="/images/mpa-hello.png" />

请注意，由于我们还没有创建任何页面，所以侧边栏不包含页面标签。

## 创建多个页面

在这里需要记住几件事情：

1. 我们可以通过在每个Python文件的开头添加数字来改变MPA中页面的排序。如果我们在文件名前面添加一个1，Streamlit会将该文件放在列表中的第一个位置。
2. 每个Streamlit应用程序的名称由文件名确定，因此要更改应用程序名称，您需要更改文件名！
3. 我们可以通过在文件名中添加表情符号来为我们的应用程序增添一些乐趣，这些表情符号将在我们的Streamlit应用程序中呈现。
4. 每个页面都有自己的URL，由文件名定义。

查看下面的示例！对于每个新页面，我们在`pages`文件夹中创建一个新文件，并将适当的演示代码添加到其中。

<br />

<details>

<summary><code>pages/1_📈_绘图演示.py</code></summary>

```python
import streamlit as st
import time
import numpy as np

st.set_page_config(page_title="Plotting Demo", page_icon="📈")

st.markdown("# Plotting Demo")
st.sidebar.header("Plotting Demo")
st.write(
    """This demo illustrates a combination of plotting and animation with
Streamlit. We're generating a bunch of random numbers in a loop for around
5 seconds. Enjoy!"""
)

progress_bar = st.sidebar.progress(0)
status_text = st.sidebar.empty()
last_rows = np.random.randn(1, 1)
chart = st.line_chart(last_rows)

for i in range(1, 101):
    new_rows = last_rows[-1, :] + np.random.randn(5, 1).cumsum(axis=0)
    status_text.text("%i%% Complete" % i)
    chart.add_rows(new_rows)
    progress_bar.progress(i)
    last_rows = new_rows
    time.sleep(0.05)

progress_bar.empty()

# Streamlit widgets automatically run the script from top to bottom. Since
# this button is not connected to any other logic, it just causes a plain
# rerun.
st.button("Re-run")
```

</details>

<Image src="/images/mpa-plotting-demo.png" />

<details>
<summary><code>pages/2_🌍_Mapping_Demo.py</code></summary>

```python
import streamlit as st
import pandas as pd
import pydeck as pdk
from urllib.error import URLError

st.set_page_config(page_title="Mapping Demo", page_icon="🌍")

st.markdown("# Mapping Demo")
st.sidebar.header("Mapping Demo")
st.write(
    """This demo shows how to use
[`st.pydeck_chart`](https://docs.streamlit.io/library/api-reference/charts/st.pydeck_chart)
to display geospatial data."""
)


@st.cache_data
def from_data_file(filename):
    url = (
        "http://raw.githubusercontent.com/streamlit/"
        "example-data/master/hello/v1/%s" % filename
    )
    return pd.read_json(url)


try:
    ALL_LAYERS = {
        "Bike Rentals": pdk.Layer(
            "HexagonLayer",
            data=from_data_file("bike_rental_stats.json"),
            get_position=["lon", "lat"],
            radius=200,
            elevation_scale=4,
            elevation_range=[0, 1000],
            extruded=True,
        ),
        "Bart Stop Exits": pdk.Layer(
            "ScatterplotLayer",
            data=from_data_file("bart_stop_stats.json"),
            get_position=["lon", "lat"],
            get_color=[200, 30, 0, 160],
            get_radius="[exits]",
            radius_scale=0.05,
        ),
        "Bart Stop Names": pdk.Layer(
            "TextLayer",
            data=from_data_file("bart_stop_stats.json"),
            get_position=["lon", "lat"],
            get_text="name",
            get_color=[0, 0, 0, 200],
            get_size=15,
            get_alignment_baseline="'bottom'",
        ),
        "Outbound Flow": pdk.Layer(
            "ArcLayer",
            data=from_data_file("bart_path_stats.json"),
            get_source_position=["lon", "lat"],
            get_target_position=["lon2", "lat2"],
            get_source_color=[200, 30, 0, 160],
            get_target_color=[200, 30, 0, 160],
            auto_highlight=True,
            width_scale=0.0001,
            get_width="outbound",
            width_min_pixels=3,
            width_max_pixels=30,
        ),
    }
    st.sidebar.markdown("### Map Layers")
    selected_layers = [
        layer
        for layer_name, layer in ALL_LAYERS.items()
        if st.sidebar.checkbox(layer_name, True)
    ]
    if selected_layers:
        st.pydeck_chart(
            pdk.Deck(
                map_style="mapbox://styles/mapbox/light-v9",
                initial_view_state={
                    "latitude": 37.76,
                    "longitude": -122.4,
                    "zoom": 11,
                    "pitch": 50,
                },
                layers=selected_layers,
            )
        )
    else:
        st.error("Please choose at least one layer above.")
except URLError as e:
    st.error(
        """
        **This demo requires internet access.**
        Connection error: %s
    """
        % e.reason
    )
```

</details>

![mpa-mapping-demo](/images/mpa-mapping-demo.png)

<details>
<summary><code>pages/3_📊_DataFrame_Demo.py</code></summary>

```python
import streamlit as st
import pandas as pd
import altair as alt
from urllib.error import URLError

st.set_page_config(page_title="DataFrame Demo", page_icon="📊")

st.markdown("# DataFrame Demo")
st.sidebar.header("DataFrame Demo")
st.write(
    """This demo shows how to use `st.write` to visualize Pandas DataFrames.
(Data courtesy of the [UN Data Explorer](http://data.un.org/Explorer.aspx).)"""
)


@st.cache_data
def get_UN_data():
    AWS_BUCKET_URL = "http://streamlit-demo-data.s3-us-west-2.amazonaws.com"
    df = pd.read_csv(AWS_BUCKET_URL + "/agri.csv.gz")
    return df.set_index("Region")


try:
    df = get_UN_data()
    countries = st.multiselect(
        "Choose countries", list(df.index), ["China", "United States of America"]
    )
    if not countries:
        st.error("Please select at least one country.")
    else:
        data = df.loc[countries]
        data /= 1000000.0
        st.write("### Gross Agricultural Production ($B)", data.sort_index())

        data = data.T.reset_index()
        data = pd.melt(data, id_vars=["index"]).rename(
            columns={"index": "year", "value": "Gross Agricultural Product ($B)"}
        )
        chart = (
            alt.Chart(data)
            .mark_area(opacity=0.3)
            .encode(
                x="year:T",
                y=alt.Y("Gross Agricultural Product ($B):Q", stack=None),
                color="Region:N",
            )
        )
        st.altair_chart(chart, use_container_width=True)
except URLError as e:
    st.error(
        """
        **This demo requires internet access.**
        Connection error: %s
    """
        % e.reason
    )
```

</details>

<Image src="/images/mpa-dataframe-demo.png" />

现在我们已经创建了额外的页面，我们可以在下面的最后一步中将它们整合在一起。

## 运行多页面应用

要运行您刚刚转换的多页面应用程序，请运行：

```bash
streamlit run Hello.py
```

就是这样！`Hello.py`脚本现在对应于您的应用程序的主页，Streamlit在pages文件夹中找到的其他脚本也将出现在侧边栏中的新页面选择器中。

<Cloud src="https://doc-mpa-hello.streamlit.app/?embed=true" height="700" />

## 下一步

恭喜！🎉如果您已经读到这里，那么很有可能您已经学会了创建单页面和多页面应用程序。您接下来的选择完全取决于您的创造力！我们很期待看到您在现在更容易添加额外页面到您的应用程序后将会构建什么。试着在我们刚刚构建的应用程序中添加更多页面作为练习。此外，欢迎到论坛与Streamlit社区分享您的多页面应用程序！🎈

下面是一些帮助您入门的资源:

- 在Streamlit的[社区云](/streamlit-community-cloud)上免费部署您的应用程序。
- 在我们的[社区论坛](https://discuss.streamlit.io/c/streamlit-examples/9)上提问或分享您的多页面应用程序。
- 查看我们的[multipage apps](/library/get-started/multipage-apps)文档。
- 阅读[高级特性](/library/advanced-features)以了解诸如缓存、主题和为应用程序添加状态的功能。
- 浏览我们的[API参考文档](/library/api-reference/)，了解每个Streamlit命令的示例。