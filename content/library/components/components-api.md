---
slug: /library/components/components-api
title: Components API
---

# 组件 API 参考

开发 Streamlit 组件的第一步是决定是创建一个静态组件（即仅渲染一次，由 Python 控制）还是创建一个双向组件，可以在 Python 和 JavaScript 之间进行通信。

## 创建静态组件

如果您创建Streamlit组件的目标仅仅是显示HTML代码或从Python可视化库渲染图表，Streamlit提供了两种方法可以大大简化这个过程：`components.html()`和`components.iframe()`。

如果您不确定是否需要双向通信，请**从这里开始**！

### 渲染HTML字符串

虽然[`st.text`](/library/api-reference/text/st.text)，[`st.markdown`](/library/api-reference/text/st.markdown)和[`st.write`](/library/api-reference/write-magic/st.write)使得在Streamlit应用程序中编写文本变得很容易，但有时您可能更喜欢实现自定义的HTML内容。同样，虽然Streamlit原生支持[许多图表库](/library/api-reference/charts#chart-elements)，但您可能希望为新的图表库实现一种特定的HTML/JavaScript模板。`components.html`的作用是允许您在Streamlit应用程序中嵌入一个包含所需输出的iframe。

`<Autofunction function="streamlit.components.v1.html" />`

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

### 渲染 iframe URL

`components.iframe`与`components.html`的功能类似，不同之处在于`components.iframe`接受URL作为输入。这用于在Streamlit应用程序中包含整个页面的情况。

<Autofunction function="streamlit.components.v1.iframe" />

**示例**

```python
import streamlit as st
import streamlit.components.v1 as components

# embed streamlit docs in a streamlit app
components.iframe("https://docs.streamlit.io/en/latest")
```

## 创建一个双向组件

双向的 Streamlit 组件由两部分组成：

1. **前端**，使用 HTML 和其他 Web 技术（如 JavaScript、React、Vue 等）构建，并通过 iframe 标签在 Streamlit 应用程序中呈现。
2. **Python API**，Streamlit 应用程序用于实例化和与前端进行通信的 API。

为了让创建双向Streamlit组件的过程更加简单，我们在[Streamlit组件模板GitHub仓库](https://github.com/streamlit/component-template)中提供了一个React模板和一个仅限于TypeScript的模板。我们还在同一仓库中提供了一些[示例组件](https://github.com/streamlit/component-template/tree/master/examples)。

### 开发环境设置

要构建一个Streamlit组件，您需要在开发环境中安装以下内容：

- Python 3.7 - Python 3.11
- Streamlit 0.63+
- [nodejs](https://nodejs.org/en/)
- [npm](https://www.npmjs.com/) 或 [yarn](https://yarnpkg.com/)

克隆[component-template GitHub仓库](https://github.com/streamlit/component-template)，然后决定是否要使用React.js（["template"](https://github.com/streamlit/component-template/tree/master/template)）或纯TypeScript（["template-reactless"](https://github.com/streamlit/component-template/tree/master/template-reactless)）模板。

1. 从终端初始化和构建组件模板的前端：

   ```bash
   # React模板
   template/my_component/frontend
   npm install    # 初始化项目并安装npm依赖
   npm run start  # 启动Webpack开发服务器

   # or

   # 只有TypeScript的模板
   template-reactless/my_component/frontend
   npm install    # 初始化项目并安装npm依赖
   npm run start  # 启动Webpack开发服务器
   ```

2. 在一个单独的终端中运行声明并使用该组件的Streamlit应用程序（Python）：

   ```bash
   # React模板
   cd template
. venv/bin/activate # 或者类似的命令激活安装了Streamlit的虚拟环境
streamlit run my_component/__init__.py # 运行示例

# 或者

# 仅TypeScript模板
cd template-reactless
. venv/bin/activate # 或者类似的命令激活安装了Streamlit的虚拟环境
streamlit run my_component/__init__.py # 运行示例
```

完成上述步骤后，您应该在浏览器中看到一个类似于以下截图的Streamlit应用程序：

![Streamlit组件示例应用程序](/images/component_demo_example.png)

模板中的示例应用展示了双向通信的实现方式。Streamlit组件显示一个按钮（`Python → JavaScript`），用户可以点击该按钮。每次点击按钮时，JavaScript前端会增加计数器的值并将其传回Python（`JavaScript → Python`），然后由Streamlit显示（`Python → JavaScript`）。

### 前端

因为每个Streamlit组件都是一个独立的网页，会被渲染到一个`iframe`中，所以您可以使用几乎任何网络技术来创建该网页。我们在Streamlit的[Components-template GitHub代码库](https://github.com/streamlit/component-template/)中提供了两个模板供您开始使用；其中一个模板使用了[React](https://reactjs.org/)，另一个没有使用。

<Note>

即使您对React还不熟悉，您仍然可以查看基于React的
模板。它处理了大部分从Streamlit发送和接收数据所需的样板文件，并且您可以边学习React的相关知识。

如果您不想使用React，请仍然阅读本节！它解释了Streamlit↔组件之间通信的基本原理。

#### React

基于React的模板位于`template/my_component/frontend/src/MyComponent.tsx`中。

- `MyComponent.render()` 在组件需要重新渲染时会自动调用（就像在任何 React 应用中一样）
- 通过 `this.props.args` 字典可以访问从 Python 脚本传递的参数。

```python
# Send arguments in Python:
result = my_component(greeting="Hello", name="Streamlit")
```

```javascript
// Receive arguments in frontend:
let greeting = this.props.args["greeting"]; // greeting = "Hello"
let name = this.props.args["name"]; // name = "Streamlit"
```

- 使用`Streamlit.setComponentValue()`将组件中的数据返回给Python脚本：

```javascript
// Set value in frontend:
Streamlit.setComponentValue(3.14);
```

```python
# Access value in Python:
result = my_component(greeting="Hello", name="Streamlit")
st.write("result = ", result) # result = 3.14
```

当您调用`Streamlit.setComponentValue(new_value)`时，这个新值会被发送到Streamlit，然后Streamlit会从头到尾重新执行Python脚本。当脚本重新执行时，对`my_component(...)`的调用将返回新的值。

从代码流的角度来看，似乎你正在以前端同步传输数据：Python将参数发送到JavaScript，JavaScript将一个值返回给Python，所有这些都在一个函数调用中完成！但实际上，这一切都是以异步的方式发生的，并且是通过重新执行Python脚本来实现这种巧妙的操作。

- 使用`Streamlit.setFrameHeight()`来控制组件的高度。默认情况下，React模板会自动调用此函数（参见`StreamlitComponentBase.componentDidUpdate()`）。如果需要更多控制，您可以覆盖此行为。
- 文件的最后一行有一些魔法：`export default withStreamlitConnection(MyComponent)` - 这会与Streamlit进行一些握手，并设置双向数据通信的机制。

#### 仅适用于TypeScript

TypeScript-only模板位于`template-reactless/my_component/frontend/src/MyComponent.tsx`。

与React版本相比，该模板代码更多，因为所有的握手机制、设置事件监听器和更新组件的框架高度都是手动完成的。React版本的模板会自动处理大部分这些细节。

- 在源文件的底部，模板调用 `Streamlit.setComponentReady()` 来告诉 Streamlit 准备好开始接收数据。（通常在创建和加载组件所依赖的内容后执行此操作。）
- 它订阅 `Streamlit.RENDER_EVENT` 以接收重绘的通知。（在调用 `setComponentReady` 之前，此事件不会被触发）
- 在其`onRender`事件处理程序中，通过`event.detail.args`访问从Python脚本传递的参数
- 它以与React模板相同的方式向Python脚本发送数据——单击“点击我！”按钮调用`Streamlit.setComponentValue()`
- 通过`Streamlit.setFrameHeight()`通知Streamlit其高度可能已更改

#### 使用主题

<Note>

自定义组件主题支持需要streamlit-component-lib版本1.2.0或更高版本。

</Note>

除了向您的组件发送一个 `args` 对象之外，Streamlit 还发送一个 `theme` 对象来定义活动主题，以便您的组件可以以兼容的方式调整样式。该对象与 `args` 一起在同一条消息中发送，因此可以通过 `this.props.theme`（使用 React 模板时）或 `event.detail.theme`（使用纯 TypeScript 模板时）访问。

`theme` 对象的结构如下：

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

`base`选项允许您指定一个预设的Streamlit主题，您的自定义主题将继承自该主题。在您的主题设置中未定义的任何主题配置选项，其值将设置为基础主题的值。`base`的有效值有`"light"`和`"dark"`。

请注意，主题对象的字段与`streamlit config show`命令打印的配置选项中的"theme"部分的选项具有相同的名称和语义。

在使用React模板时，以下CSS变量也会自动设置。

```css
--base
--primary-color
--background-color
--secondary-background-color
--text-color
--font
```

如果您对[CSS变量](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)不熟悉，简单来说，您可以像这样使用它们:

```css
.mySelector {
  color: var(--text-color);
}
```

这些变量与上面定义的`theme`对象中的字段相匹配，选择在组件中使用CSS变量还是主题对象是个人偏好的问题。

#### 其他前端细节

- 因为您是通过开发服务器（通过`npm run start`）托管组件，所以在保存时对所做的任何更改应该会自动反映在Streamlit应用程序中。
- 如果您想要向组件添加更多的软件包，请在组件的 `frontend/` 目录中运行 `npm add` 来添加它们。

```bash
npm add baseui
```

- 要构建组件的静态版本，请运行`npm run build`。有关更多信息，请参见[准备您的组件](publish#prepare-your-component)

### Python API

只需使用`components.declare_component()`即可创建您的组件的Python API：

```python
  import streamlit.components.v1 as components
  my_component = components.declare_component(
    "my_component",
    url="http://localhost:3001"
  )
```

然后，您可以使用返回的`my_component`函数来与前端代码进行数据的发送和接收：

```python
# Send data to the frontend using named arguments.
return_value = my_component(name="Blackbeard", ship="Queen Anne's Revenge")

# `my_component`'s return value is the data returned from the frontend.
st.write("Value = ", return_value)
```

尽管上述内容是您在Python端定义一个可工作组件所需的全部内容，但我们建议创建一个“包装器”函数，该函数具有命名参数和默认值、输入验证等。这将使最终用户更容易理解您的函数接受的数据值，并允许定义有用的文档字符串。

请参考[这个示例](https://github.com/streamlit/component-template/blob/master/template/my_component/__init__.py#L41-L77)，这是一个创建包装函数的示例。

### 数据序列化

#### Python → 前端

通过将关键字参数传递给组件的调用函数（即从`declare_component`返回的函数），您可以将数据从Python发送到前端。您可以从Python发送以下类型的数据到前端：

- 任何可JSON序列化的数据
- `numpy.array`
- `pandas.DataFrame`

任何可JSON序列化的数据都被序列化为JSON字符串，并反序列化为其对应的JavaScript表示。`numpy.array`和`pandas.DataFrame`使用[Apache Arrow](https://arrow.apache.org/)进行序列化，并作为`ArrowTable`的实例进行反序列化，`ArrowTable`是一个自定义类型，包装了Arrow结构并提供了一个方便的API。

查看[CustomDataframe](https://github.com/streamlit/component-template/tree/master/examples/CustomDataframe)和[SelectableDataTable](https://github.com/streamlit/component-template/tree/master/examples/SelectableDataTable)组件示例代码，以获取更多关于如何使用`ArrowTable`的上下文信息。

#### 前端 → Python

您可以通过`Streamlit.setComponentValue()` API将数据从前端发送到Python（这是模板代码的一部分）。与从Python →前端的参数传递不同，**此API接受单个值**。如果您想返回多个值，您需要将它们包装在一个`Array`或`Object`中。

自定义组件可以将JSON可序列化的数据从前端发送到Python，以及用于表示数据框的[Apache Arrow](http://arrow.apache.org/) `ArrowTable`。