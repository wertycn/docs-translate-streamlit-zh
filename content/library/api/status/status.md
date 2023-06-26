---
slug: /library/api-reference/status
title: Display progress and status
---

# 显示进度和状态

Streamlit提供了一些方法，可以让您为应用程序添加动画效果。这些动画效果包括进度条、状态消息（如警告）和庆祝气球。

<TileContainer>
<RefCard href="/library/api-reference/status/st.progress">

<Image pure alt="screenshot" src="/images/api/progress.jpg" />

#### 进度条

显示一个进度条。

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

展示庆祝的气球！

```python
do_something()

# Celebrate when all done!
st.balloons()
```

</RefCard>
<RefCard href="/library/api-reference/status/st.snow">

<Image pure alt="截图" src="/images/api/snow.jpg" />

#### 雪花

展示庆祝用的雪花！

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

显示一条信息性的消息。

```python
st.info("Dataset is updated every day at midnight.")
```

</RefCard>
<RefCard href="/library/api-reference/status/st.success">

<Image pure alt="screenshot" src="/images/api/success.jpg" />

#### 成功框

显示成功消息。

```python
st.success("Match found!")
```

</RefCard>
<RefCard href="/library/api-reference/status/st.exception">

<Image pure alt="screenshot" src="/images/api/exception.jpg" />

#### 异常输出

显示一个异常。

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

在Streamlit应用程序中处理进度条的最简单方式。由[@Wirg](https://github.com/Wirg)创建。

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

一个包含有用的Streamlit额外功能的库。由[@arnaudmiribel](https://github.com/arnaudmiribel/)创建。

```python
from streamlit_extras.let_it_rain import rain

rain(emoji="🎈", font_size=54,
  falling_speed=5, animation_length="infinite",)
```

</ComponentCard>

</ComponentSlider>