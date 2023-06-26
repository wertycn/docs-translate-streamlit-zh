---
slug: /library/api-reference/media
title: Media elements
---

# 媒体元素

在Streamlit应用程序中直接嵌入图像、视频和音频文件非常简单。

<TileContainer>
<RefCard href="/library/api-reference/media/st.image">

<Image pure alt="screenshot" src="/images/api/image.jpg" />

#### 图像

显示一个图像或图像列表。

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

使用Streamlit处理和传输实时视频/音频流。由[@whitphx](https://github.com/whitphx)创建。

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

#### 图片比较

使用[JuxtaposeJS](https://juxtapose.knightlab.com/)的滑块来比较图片。由[@fcakyon](https://github.com/fcakyon)创建。

```python
from streamlit_image_comparison import image_comparison

image_comparison(img1="image1.jpg", img2="image2.jpg",)
```

</ComponentCard>

<ComponentCard href="https://github.com/turner-anderson/streamlit-cropper">

<Image pure alt="screenshot" src="/images/api/components/cropper.jpg" />

#### Streamlit图片裁剪工具

一个简单的用于Streamlit的图片裁剪工具。由[@turner-anderson](https://github.com/turner-anderson)创建。

```python
from streamlit_cropper import st_cropper

st_cropper(img, realtime_update=realtime_update, box_color=box_color, aspect_ratio=aspect_ratio)
```

</ComponentCard>

<ComponentCard href="https://github.com/blackary/streamlit-image-coordinates">

<Image pure alt="screenshot" src="/images/api/components/image-coordinates.jpg" />

#### 图像坐标

获取图像上点击的坐标。由[@blackary](https://github.com/blackary/)创建。

```python
from streamlit_image_coordinates import streamlit_image_coordinates

streamlit_image_coordinates("https://placekitten.com/200/300")
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

</ComponentSlider>