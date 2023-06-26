---
title: 组件 API
slug: /library/components/components-api
---

# 组件 API 参考

开发 Streamlit 组件的第一步是决定是创建一个静态组件（即由 Python 控制的一次渲染）还是创建一个双向组件，可以在 Python 和 JavaScript 之间进行通信。

## 创建一个静态组件

如果您创建Streamlit组件的目标仅仅是显示HTML代码或从Python可视化库中呈现图表，Streamlit提供了两种方法大大简化了这个过程：`components.html()`和`components.iframe()`。

如果您不确定是否需要双向通信，请**首先从这里开始**！

### 渲染HTML字符串

虽然 [`st.text`](/library/api-reference/text/st.text)，[`st.markdown`](/library/api-reference/text/st.markdown) 和 [`st.write`](/library/api-reference/write-magic/st.write) 可以轻松地将文本写入 Streamlit 应用程序，但有时您可能更喜欢实现自定义的 HTML 片段。同样地，虽然 Streamlit 本身支持[许多图表库](/library/api-reference/charts#chart-elements)，但您可能想要为新的图表库实现一个特定的 HTML/JavaScript 模板。通过 `components.html`，您可以在 Streamlit 应用程序中嵌入一个包含所需输出的 iframe。

<Autofunction function="streamlit.components.v1.html" />

**示例**

```python
import streamlit as st
import streamlit.components.v1 as components

# bootstrap 4 collapse example
components.html(
    """
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
    <div id="accordion">
      <div class="card">
        <div class="card-header" id="headingOne">
          <h5 class="mb-0">
            <button class="btn btn-link" data-toggle="collapse" data-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
            Collapsible Group Item #1
            </button>
          </h5>
        </div>
        <div id="collapseOne" class="collapse show" aria-labelledby="headingOne" data-parent="#accordion">
          <div class="card-body">
            Collapsible Group Item #1 content
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header" id="headingTwo">
          <h5 class="mb-0">
            <button class="btn btn-link collapsed" data-toggle="collapse" data-target="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
            Collapsible Group Item #2
            </button>
          </h5>
        </div>
        <div id="collapseTwo" class="collapse" aria-labelledby="headingTwo" data-parent="#accordion">
          <div class="card-body">
            Collapsible Group Item #2 content
          </div>
        </div>
      </div>
    </div>
    """,
    height=600,
)
```

### 嵌入 iframe URL

`components.iframe` 和 `components.html` 的功能类似，但不同之处在于 `components.iframe` 接受一个 URL 作为输入。这在您想要在 Streamlit 应用程序中包含整个页面的情况下非常有用。

<Autofunction function="streamlit.components.v1.iframe" />

**示例**

```python
import streamlit as st
import streamlit.components.v1 as components

# embed streamlit docs in a streamlit app
components.iframe("https://docs.streamlit.io/en/latest")
```

## 创建一个双向组件

一个双向的Streamlit组件由两部分组成：

1. **前端**，由HTML和其他任何您喜欢的Web技术（JavaScript，React，Vue等）构建，并通过iframe标签在Streamlit应用程序中渲染。
2. **Python API**，Streamlit应用程序用于实例化和与前端进行通信。

为了简化创建双向Streamlit组件的过程，我们在[Streamlit组件模板GitHub仓库](https://github.com/streamlit/component-template)中创建了一个React模板和一个仅限TypeScript的模板。我们还在同一仓库中提供了一些[示例组件](https://github.com/streamlit/component-template/tree/master/examples)。

### 开发环境设置

要构建一个Streamlit组件，您的开发环境需要安装以下内容：

- Python 3.7 - Python 3.11
- Streamlit 0.63+
- [nodejs](https://nodejs.org/en/)
- [npm](https://www.npmjs.com/) 或 [yarn](https://yarnpkg.com/)

克隆[component-template GitHub仓库](https://github.com/streamlit/component-template)，然后决定您是想使用React.js（["template"](https://github.com/streamlit/component-template/tree/master/template)）还是纯TypeScript（["template-reactless"](https://github.com/streamlit/component-template/tree/master/template-reactless)）模板。

1. 从终端初始化并构建组件模板前端：

   ```bash
   # React模板
   template/my_component/frontend
   npm install    # 初始化项目并安装npm依赖
   npm run start  # 启动Webpack开发服务器

   # 或者

   # 仅限TypeScript模板
   template-reactless/my_component/frontend
   npm install    # 初始化项目并安装npm依赖
   npm run start  # 启动Webpack开发服务器
   ```

2. _从一个单独的终端_中，运行声明并使用该组件的Streamlit应用程序（Python）：

   ```bash
   # React模板
   ```shell
cd template
. venv/bin/activate # 或者类似的命令激活安装了Streamlit的虚拟环境
streamlit run my_component/__init__.py # 运行示例

# 或者

# 仅包含TypeScript的模板
cd template-reactless
. venv/bin/activate # 或者类似的命令激活安装了Streamlit的虚拟环境
streamlit run my_component/__init__.py # 运行示例
```

在运行上述步骤后，您应该在浏览器中看到一个类似于下面这样的Streamlit应用程序:

![Streamlit组件示例应用程序](/images/component_demo_example.png)

模板中的示例应用程序展示了双向通信的实现方式。Streamlit组件显示了一个按钮（`Python → JavaScript`），终端用户可以点击该按钮。每次点击按钮时，JavaScript前端会递增计数器的值并将其传递回Python（`JavaScript → Python`），然后由Streamlit（`Python → JavaScript`）显示出来。

### 前端部分

因为每个Streamlit组件都是一个独立的网页，会被渲染到一个`iframe`中，所以您可以使用几乎任何网络技术来创建这个网页。我们在Streamlit的[Components-template GitHub存储库](https://github.com/streamlit/component-template/)中提供了两个模板供您开始使用；其中一个模板使用了[React](https://reactjs.org/)，另一个没有使用。

<Note>

即使您对React还不熟悉，您仍然可以查看基于React的模板，因为它提供了一些额外的功能和灵活性。
模板。它处理了与Streamlit发送和接收数据所需的大部分样板代码，您可以在使用过程中学习React的相关知识。

如果您不想使用React，请仍然阅读本节！它解释了Streamlit ↔ Component通信的基本原理。
</Note>

#### React

基于React的模板位于 `template/my_component/frontend/src/MyComponent.tsx`。

- `MyComponent.render()` 在组件需要重新渲染时会自动调用（就像在任何 React 应用中一样）
- 从 Python 脚本传递的参数可以通过 `this.props.args` 字典进行访问：

```python
# Send arguments in Python:
result = my_component(greeting="Hello", name="Streamlit")
```

```javascript
// Receive arguments in frontend:
let greeting = this.props.args["greeting"]; // greeting = "Hello"
let name = this.props.args["name"]; // name = "Streamlit"
```

- 使用`Streamlit.setComponentValue()`将组件中的数据返回给Python脚本:

```javascript
// Set value in frontend:
Streamlit.setComponentValue(3.14);
```

```python
# Access value in Python:
result = my_component(greeting="Hello", name="Streamlit")
st.write("result = ", result) # result = 3.14
```

当您调用`Streamlit.setComponentValue(new_value)`时，该新值将被发送到Streamlit，然后Streamlit会从头到尾重新执行Python脚本。当脚本被重新执行时，对`my_component(...)`的调用将返回新值。

从代码流的角度来看，似乎你正在同步地传输数据给前端：Python将参数发送给JavaScript，JavaScript返回一个值给Python，所有这些都在一个函数调用中完成！但实际上，这一切都是异步发生的，而Python脚本的重新执行正是做到这一点的诀窍。

- 使用`Streamlit.setFrameHeight()`来控制组件的高度。默认情况下，React模板会自动调用它（参见`StreamlitComponentBase.componentDidUpdate()`）。如果需要更多的控制，您可以覆盖这个行为。
- 在文件的最后一行有一点点魔力：`export default withStreamlitConnection(MyComponent)` - 这与Streamlit进行了一些握手，并设置了双向数据通信的机制。

#### 仅限TypeScript

TypeScript-only模板位于 `template-reactless/my_component/frontend/src/MyComponent.tsx`。

与React版本相比，这个模板的代码要多得多，因为它需要手动完成握手、设置事件监听器和更新组件的框架高度等机制。而React版本的模板则会自动处理大部分这些细节。

- 在源文件的底部，模板调用 `Streamlit.setComponentReady()` 来告诉 Streamlit 准备好开始接收数据。（通常在创建和加载组件所依赖的内容后执行此操作。）
- 它订阅 `Streamlit.RENDER_EVENT`，以便在需要重新绘制时收到通知。（在调用 `setComponentReady` 之前，此事件不会触发）
- 在其`onRender`事件处理程序中，通过`event.detail.args`访问从Python脚本传入的参数
- 通过与React模板相同的方式将数据发送回Python脚本——单击“Click Me!”按钮调用`Streamlit.setComponentValue()`
- 通过`Streamlit.setFrameHeight()`通知Streamlit其高度可能已更改

#### 使用主题

<Note>

自定义组件主题支持需要streamlit-component-lib版本1.2.0或更高版本。

</Note>

除了向组件发送一个`args`对象之外，Streamlit还会发送一个`theme`对象，用于定义活动主题，以便您的组件可以以兼容的方式调整样式。该对象与`args`一起在同一条消息中发送，因此可以通过`this.props.theme`（使用React模板）或`event.detail.theme`（使用普通TypeScript模板）进行访问。

`theme`对象的结构如下：

```json
{
  "base": "lightORdark",
  "primaryColor": "someColor1",
  "backgroundColor": "someColor2",
  "secondaryBackgroundColor": "someColor3",
  "textColor": "someColor4",
  "font": "someFont"
}
```

`base`选项允许您指定一个预设的Streamlit主题，您的自定义主题将继承自该主题。在主题设置中未定义的任何主题配置选项都将使用基础主题的值。`base`的有效值为`"light"`和`"dark"`。

请注意，主题对象具有与命令`streamlit config show`打印的配置选项中的"theme"部分具有相同的名称和语义的字段。

在使用React模板时，以下CSS变量也会自动设置。

```css
--base
--primary-color
--background-color
--secondary-background-color
--text-color
--font
```

如果您对[CSS变量](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)还不熟悉，简而言之，您可以像这样使用它们：

```css
.mySelector {
  color: var(--text-color);
}
```

这些变量与上面定义的`theme`对象中的字段匹配，使用CSS变量还是组件中的主题对象取决于个人偏好。

#### 其他前端细节

- 因为您正在从开发服务器（通过`npm run start`）托管组件，所以在保存时对所做的任何更改应该会自动反映在Streamlit应用程序中。
- 如果您想在组件中添加更多的包，可以在组件的`frontend/`目录中运行`npm add`来添加它们。

```bash
npm add baseui
```

- 要构建组件的静态版本，请运行`npm run build`。有关更多信息，请参阅[准备组件](publish#prepare-your-component)。

### Python API

只需使用`components.declare_component()`即可创建组件的Python API：

```python
  import streamlit.components.v1 as components
  my_component = components.declare_component(
    "my_component",
    url="http://localhost:3001"
  )
```

您可以使用返回的 `my_component` 函数来在前端代码中发送和接收数据：

```python
# Send data to the frontend using named arguments.
return_value = my_component(name="Blackbeard", ship="Queen Anne's Revenge")

# `my_component`'s return value is the data returned from the frontend.
st.write("Value = ", return_value)
```

尽管上面是在Python端定义一个工作组件所需的全部内容，但我们建议创建一个带有命名参数和默认值、输入验证等的“包装器”函数。这将使最终用户更容易理解您的函数接受的数据值，并允许定义有用的文档字符串。

请参考[这个示例](https://github.com/streamlit/component-template/blob/master/template/my_component/__init__.py#L41-L77)，该示例演示了如何创建一个包装函数。

### 数据序列化

#### Python → 前端

您可以通过将关键字参数传递给组件的调用函数（即从`declare_component`返回的函数）将数据从Python发送到前端。您可以从Python发送以下类型的数据到前端：

- 任何可JSON序列化的数据
- `numpy.array`
- `pandas.DataFrame`

任何可JSON序列化的数据都会被序列化为JSON字符串，并反序列化为其JavaScript等效形式。`numpy.array`和`pandas.DataFrame`使用[Apache Arrow](https://arrow.apache.org/)进行序列化，并作为`ArrowTable`的实例进行反序列化。`ArrowTable`是一个自定义类型，用于封装Arrow结构并在其之上提供便捷的API。

请查看[CustomDataframe](https://github.com/streamlit/component-template/tree/master/examples/CustomDataframe)和[SelectableDataTable](https://github.com/streamlit/component-template/tree/master/examples/SelectableDataTable)的组件示例代码，以获取有关如何使用`ArrowTable`的更多上下文信息。

#### 前端 → Python

您可以通过`Streamlit.setComponentValue()` API（这是模板代码的一部分）将数据从前端发送到Python。与从Python →前端的参数传递不同，**这个API接受一个单一的值**。如果您想返回多个值，您需要将它们包装在一个`Array`或`Object`中。

自定义组件可以将JSON可序列化的数据从前端发送到Python，还可以使用[Apache Arrow](http://arrow.apache.org/)的`ArrowTable`来表示数据帧。
