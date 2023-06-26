---
slug: /library/api-reference/text
title: Text elements
---

# 文本元素

Streamlit应用程序通常从调用`st.title`开始，以设置应用程序的标题。之后，您可以使用两个标题级别：`st.header`和`st.subheader`。

使用`st.text`输入纯文本，使用`st.markdown`输入Markdown。

我们还提供了一个名为`st.write`的“瑞士军刀”命令，它接受多个参数和多个数据类型。如上所述，您还可以在`st.write`的位置使用[魔法命令](/library/api-reference/write-magic/magic)。

<瓷砖容器>
<RefCard href="/library/api-reference/text/st.markdown">

<Image pure alt="屏幕截图" src="/images/api/markdown.jpg" />

#### Markdown

以Markdown格式显示的字符串。

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

#### 标题

以标题格式显示文本。

```python
st.header("This is a header")
```

</RefCard>
<RefCard href="/library/api-reference/text/st.subheader">

<Image pure alt="screenshot" src="/images/api/subheader.jpg" />

#### Subheader

Display text in subheader formatting.

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

#### 预格式化文本

编写固定宽度和预格式化文本。

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

#### 分割线

显示一条水平的分割线。

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

在您的Streamlit应用程序中添加标签。由[@gagan3012](https://github.com/gagan3012)创建。

```python
st_tags(label='# Enter Keywords:', text='Press enter to add more', value=['Zero', 'One', 'Two'], suggestions=['five', 'six', 'seven', 'eight', 'nine', 'three', 'eleven', 'ten', 'four'], maxtags = 4, key='1')
```

</ComponentCard>

<ComponentCard href="https://github.com/JohnSnowLabs/nlu">

<Image pure alt="screenshot" src="/images/api/components/nlu.jpg" />

#### NLU

在数据框上应用文本挖掘。由[@JohnSnowLabs](https://github.com/JohnSnowLabs/)创建。

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