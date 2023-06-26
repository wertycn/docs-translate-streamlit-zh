---
title: st.cache_resource
slug: /library/api-reference/performance/st.cache_resource
description: st.cache_resource用于缓存返回共享全局资源的函数（例如数据库连接，机器学习模型）。

<Tip>

此页面仅包含有关`st.cache_resource` API的信息。要深入了解缓存及其使用方法，请查看[Caching](/library/advanced-features/caching)。
</Tip>

<Autofunction function="streamlit.cache_resource" />

## 在缓存函数中使用 Streamlit 命令

### 静态元素

自 1.16.0 版本以来，缓存函数可以包含 Streamlit 命令！例如，您可以这样做：

```python
from transformers import pipeline

@st.cache_resource
def load_model():
    model = pipeline("sentiment-analysis")
    st.success("Loaded NLP model from Hugging Face!")  # 👈 Show a success message
    return model
```

正如我们所知，Streamlit只有在之前没有缓存时才会运行此函数。在第一次运行时，`st.success`消息将显示在应用程序中。但是在后续运行中会发生什么呢？它仍然显示！Streamlit意识到在缓存的函数内部存在`st.`命令，它在第一次运行时保存它，并在后续运行中重新播放它。重新播放静态元素对于所有缓存装饰器都可以工作。

您还可以使用此功能来缓存整个UI的部分:

```python
@st.cache_resource
def load_model():
    st.header("Data analysis")
    model = torchvision.models.resnet50(weights=ResNet50_Weights.DEFAULT)
    st.success("Loaded model!")
    st.write("Turning on evaluation mode...")
    model.eval()
    st.write("Here's the model:")
    return model
```

### 输入小部件

您还可以在缓存函数中使用 [交互式输入小部件](/library/api-reference/widgets)，例如 `st.slider` 或 `st.text_input`。小部件回放是一个实验性的功能。要启用它，您需要设置 `experimental_allow_widgets` 参数：

```python
@st.cache_data(experimental_allow_widgets=True)  # 👈 Set the parameter
def load_model():
    pretrained = st.checkbox("Use pre-trained model:")  # 👈 Add a checkbox
    model = torchvision.models.resnet50(weights=ResNet50_Weights.DEFAULT, pretrained=pretrained)
    return model
```

Streamlit将复选框视为缓存函数的附加输入参数。如果您取消选中它，Streamlit将检查是否已经为此复选框状态缓存了函数。如果是，则返回缓存的值。如果没有，则使用新的滑块值重新运行函数。

在缓存函数中使用小部件非常强大，因为它可以缓存应用程序的整个部分。但这也有一定的风险！由于Streamlit将小部件值视为额外的输入参数，它很容易导致内存使用过多。想象一下，如果您的缓存函数有五个滑块，并返回一个100MB的DataFrame。那么对于这五个滑块值的每一种组合，我们都会将100MB添加到缓存中，即使这些滑块不会影响返回的数据！这些添加操作会使您的缓存迅速膨胀。如果您在缓存函数中使用小部件，请注意这个限制。我们建议仅在小部件直接影响缓存返回值的隔离部分使用此功能。

<警告>

对于缓存函数中的小部件的支持目前处于实验阶段。我们可能随时更改或删除它，而不提供警告。请谨慎使用！
</警告>

<注意>

目前不支持在缓存函数中使用两个小部件：`st.file_uploader`和`st.camera_input`。我们可能会在未来支持它们。如果您需要它们，请随时[提交一个 GitHub 问题](https://github.com/streamlit/streamlit/issues)！
</注意>
