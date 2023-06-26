---
slug: /library/api-reference/widgets
title: Input widgets
---

# 输入小部件

使用小部件，Streamlit允许您在应用程序中直接添加按钮、滑块、文本输入框等交互功能。

<TileContainer>
<RefCard href="/library/api-reference/widgets/st.button">

<Image pure alt="screenshot" src="/images/api/button.jpg" />

#### 按钮

显示一个按钮小部件。

```python
clicked = st.button("Click me")
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

#### 单选框

显示一个单选框小部件。

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

显示一个多选小部件。多选小部件初始为空。

```python
choices = st.multiselect("Buy", ["milk", "apples", "potatoes"])
```

</RefCard>
<RefCard href="/library/api-reference/widgets/st.slider">

<Image pure alt="screenshot" src="/images/api/slider.jpg" />

#### 滑动条

显示一个滑动条小部件。

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

#### 摄像头输入

显示一个小部件，允许用户直接从摄像头上传图片。

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

在Streamlit中创建一个可拖动和可调整大小的仪表盘。由[@okls](https://github.com/okls)创建。

```python
from streamlit_elements import elements, mui, html

with elements("new_element"):
  mui.Typography("Hello world")
```

</ComponentCard>

<ComponentCard href="https://github.com/gagan3012/streamlit-tags">

<Image pure alt="screenshot" src="/images/api/components/tags.jpg" />

#### 标签

为您的Streamlit应用添加标签。由[@gagan3012](https://github.com/gagan3012)创建。

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

<Image pure alt="截图" src="/images/api/components/timeline.jpg" />

#### 时间线

使用[TimelineJS](https://timeline.knightlab.com/)在Streamlit应用程序中显示时间线。由[@innerdoc](https://github.com/innerdoc)创建。

```python
from streamlit_timeline import timeline

with open('example.json', "r") as f:
  timeline(f.read(), height=800)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/streamlit-camera-input-live">

<Image pure alt="screenshot" src="/images/api/components/camera-live.jpg" />

#### 实时相机输入

st.camera_input的替代品，以实时方式返回网络摄像头图像。由[@blackary](https://github.com/blackary)创建。

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

Streamlit的聊天界面组件。由[@AI-Yash](https://github.com/AI-Yash)创建。

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

一个包含有用的Streamlit扩展的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
from streamlit_extras.stoggle import stoggle

stoggle(
    "Click me!", """🥷 Surprise! Here's some additional content""",)
```

</ComponentCard>

</ComponentSlider>