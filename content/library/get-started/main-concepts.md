---
slug: /library/get-started/main-concepts
title: Main concepts
---

# 主要概念

使用Streamlit非常简单。首先，在普通的Python脚本中添加几个Streamlit命令，然后使用`streamlit run`来运行它:

```bash
streamlit run your_script.py [-- script args]
```

当你按照上述方式运行脚本后，一个本地的Streamlit服务器将会启动，并且你的应用程序将在默认的网络浏览器中以新标签页的形式打开。应用程序是你的画布，在这里你可以绘制图表、文本、小部件、表格等等。

应用程序中绘制的内容由你决定。例如，[`st.text`](/library/api-reference/text/st.text) 可以将原始文本写入你的应用程序，而 [`st.line_chart`](/library/api-reference/charts/st.line_chart) 可以绘制线形图。
线图。请参阅我们的[API文档](/library/api-reference)以查看所有可用的命令。

<注意>

当为脚本传递自定义参数时，它们必须在两个破折号之后传递。否则，这些参数将被解释为Streamlit本身的参数。

</注意>

另一种运行Streamlit的方式是将其作为Python模块运行。这在配置像PyCharm这样的IDE与Streamlit一起使用时非常有用：

```bash
# Running
python -m streamlit run your_script.py

# is equivalent to:
streamlit run your_script.py
```

<提示>

您还可以将URL传递给`streamlit run`！当与GitHub Gists结合使用时，效果非常好。例如：

```bash
streamlit run https://raw.githubusercontent.com/streamlit/demo-uber-nyc-pickups/master/streamlit_app.py
```

</Tip>

## 开发流程

每当您想要更新应用程序时，请保存源文件。在这样做时，Streamlit会检测是否有更改，并询问您是否要重新运行应用程序。在屏幕右上角选择“始终重新运行”，以在每次更改源代码时自动更新应用程序。

这样可以让您在一个快速的交互循环中工作：您键入一些代码，保存它，实时尝试它，然后键入更多代码，保存它，再次尝试，以此类推。
直到您对结果满意为止。在编码和实时查看结果之间的紧密循环是Streamlit使您的生活更轻松的方式之一。

<提示>

在开发Streamlit应用程序时，建议将编辑器和浏览器窗口并排放置，以便同时查看代码和应用程序。试试看！

</提示>

从Streamlit 1.10.0版本开始，无法在Linux发行版的根目录中运行Streamlit应用程序。如果您尝试从根目录运行Streamlit应用程序，Streamlit将抛出`FileNotFoundError: [Errno 2] No such file or directory`错误。更多信息，请参见GitHub问题[#5239](https://github.com/streamlit/streamlit/issues/5239)。

如果您使用的是Streamlit 1.10.0或更高版本，您的主要脚本应该位于与根目录不同的目录中。使用Docker时，可以使用`WORKDIR`命令来指定您的主要脚本所在的目录。有关如何执行此操作的示例，请阅读[创建Dockerfile](/knowledge-base/tutorials/deploy/docker#create-a-dockerfile)。

## 数据流

Streamlit的架构允许您以与编写纯文本文件相同的方式编写应用程序。
Python脚本。为了解锁这个功能，Streamlit应用具有独特的数据流：每当屏幕上的某个内容需要更新时，Streamlit会从顶部到底部重新运行整个Python脚本。

这种情况可能发生在以下两种情况下：

- 每当您修改应用程序的源代码时。

- 每当用户与应用程序中的小部件进行交互时。例如，拖动滑块、在输入框中输入文本或点击按钮。

每当通过`on_change`（或`on_click`）参数将回调函数传递给小部件时，回调函数将始终在脚本的其余部分之前运行。有关回调函数 API 的详细信息，请参阅我们的[会话状态 API 参考指南](/library/api-reference/session-state#use-callbacks-to-update-session-state)。

为了使所有这些变得快速和无缝，Streamlit在幕后为您做了一些重要的工作。在这个故事中，一个重要角色是
[`@st.cache_data`](#caching) 装饰器允许开发者在应用程序重新运行时跳过某些昂贵的计算。我们将在本页面后面介绍缓存。

## 显示和风格化数据

在 Streamlit 应用程序中，有几种方法可以显示数据（表格、数组、数据框）。在下面的链接中，您将了解到 _magic_ 和 [`st.write()`](/library/api-reference/write-magic/st.write)，它们可以用于写入数据。
### 使用魔术命令

您可以在不调用任何Streamlit方法的情况下编写应用程序。Streamlit支持"[魔术命令](/library/api-reference/write-magic/magic)"，这意味着您不需要使用[`st.write()`](/library/api-reference/write-magic/st.write)。要查看它的效果，请尝试以下代码片段：

```python
"""
# My first app
Here's our first attempt at using data to create a table:
"""

import streamlit as st
import pandas as pd
df = pd.DataFrame({
  'first column': [1, 2, 3, 4],
  'second column': [10, 20, 30, 40]
})

df
```

每当Streamlit在单独的一行中看到一个变量或字面值时，它会自动使用[`st.write()`](/library/api-reference/write-magic/st.write)将其写入您的应用程序。有关更多信息，请参阅[magic commands](/library/api-reference/write-magic/magic)的文档。

### 写入数据框

除了[magic commands](/library/api-reference/write-magic/magic)之外，[`st.write()`](/library/api-reference/write-magic/st.write)是Streamlit的"瑞士军刀"。
可以将几乎任何内容传递给 [`st.write()`](/library/api-reference/write-magic/st.write) 函数：文本、数据、Matplotlib 图表、Altair 图表等等。不用担心，Streamlit 会自动识别并以正确的方式渲染内容。

```python
import streamlit as st
import pandas as pd

st.write("Here's our first attempt at using data to create a table:")
st.write(pd.DataFrame({
    'first column': [1, 2, 3, 4],
    'second column': [10, 20, 30, 40]
}))
```

还有其他特定于数据的函数，比如[`st.dataframe()`](/library/api-reference/data/st.dataframe)和[`st.table()`](/library/api-reference/data/st.table)，您也可以用它们来显示数据。让我们了解何时使用这些功能以及如何向数据框添加颜色和样式。

您可能会问自己，“为什么我不总是使用`st.write()`？”有几个原因：

1. _Magic_ 和 [`st.write()`](/library/api-reference/write-magic/st.write) 检查类型
   1. 第一个原因是，使用不同的Streamlit方法可以根据您传递的数据来决定最佳的渲染方式。有时候您可能希望以其他方式绘制数据。例如，您可以使用`st.table(df)`将数据框绘制为静态表格，而不是交互式表格。
2. 第二个原因是，其他方法返回一个可以使用和修改的对象，可以向其添加数据或替换数据。
3. 最后，如果您使用更具体的Streamlit方法，还可以传递其他附加参数。
   用于自定义其行为的参数。

例如，让我们创建一个数据框并使用Pandas的`Styler`对象更改其格式。在这个例子中，您将使用Numpy来生成一个随机样本，以及[`st.dataframe()`](/library/api-reference/data/st.dataframe)方法来绘制一个交互式表格。

<注意>

这个例子使用Numpy来生成一个随机样本，但是您也可以使用Pandas的数据框、Numpy数组或纯Python数组。

</注意>

```python
import streamlit as st
import numpy as np

dataframe = np.random.randn(10, 20)
st.dataframe(dataframe)
```

让我们扩展第一个示例，使用Pandas的`Styler`对象来突出显示交互式表格中的一些元素。

```python
import streamlit as st
import numpy as np
import pandas as pd

dataframe = pd.DataFrame(
    np.random.randn(10, 20),
    columns=('col %d' % i for i in range(20)))

st.dataframe(dataframe.style.highlight_max(axis=0))
```

Streamlit还提供了一种生成静态表格的方法：[`st.table()`](/library/api-reference/data/st.table)。

```python
import streamlit as st
import numpy as np
import pandas as pd

dataframe = pd.DataFrame(
    np.random.randn(10, 20),
    columns=('col %d' % i for i in range(20)))
st.table(dataframe)
```

### 绘制图表和地图

Streamlit支持多种流行的数据图表库，如[Matplotlib，Altair，deck.gl等](/library/api-reference#chart-elements)。在本节中，您将向应用程序中添加一个条形图、折线图和地图。

### 绘制折线图

您可以使用[`st.line_chart()`](/library/api-reference/charts/st.line_chart)轻松地向应用程序中添加折线图。我们将使用Numpy生成一个随机样本，然后将其绘制成图表。

```python
import streamlit as st
import numpy as np
import pandas as pd

chart_data = pd.DataFrame(
     np.random.randn(20, 3),
     columns=['a', 'b', 'c'])

st.line_chart(chart_data)
```

### 绘制地图

使用[`st.map()`](/library/api-reference/charts/st.map)可以在地图上显示数据点。
让我们使用Numpy生成一些示例数据，并在旧金山的地图上进行绘制。

```python
import streamlit as st
import numpy as np
import pandas as pd

map_data = pd.DataFrame(
    np.random.randn(1000, 2) / [50, 50] + [37.76, -122.4],
    columns=['lat', 'lon'])

st.map(map_data)
```

## 小部件

当您将数据或模型准备好进行探索时，您可以添加小部件，例如 [`st.slider()`](/library/api-reference/widgets/st.slider)、[`st.button()`](/library/api-reference/widgets/st.button) 或 [`st.selectbox()`](/library/api-reference/widgets/st.selectbox)。这非常简单 - 将小部件视为变量：

```python
import streamlit as st
x = st.slider('x')  # 👈 this is a widget
st.write(x, 'squared is', x * x)
```

第一次运行时，上面的应用程序应该输出文本"0的平方是0"。然后，每当用户与小部件进行交互时，Streamlit会简单地从头到尾重新运行您的脚本，并将小部件的当前状态赋值给您的变量。

例如，如果用户将滑块移动到位置`10`，Streamlit将重新运行上述代码，并相应地将`x`设置为`10`。因此，现在您应该看到文本"10的平方是100"。

如果您选择指定一个字符串作为小部件的唯一键，也可以通过键访问小部件：

```python
import streamlit as st
st.text_input("Your name", key="name")

# You can access the value at any point with:
st.session_state.name
```

每个具有键的小部件将自动添加到会话状态中。有关会话状态、其与小部件状态的关联以及其限制的更多信息，请参阅[会话状态 API 参考指南](/library/api-reference/session-state)。

### 使用复选框来显示/隐藏数据

复选框的一个用例是在应用程序中隐藏或显示特定的图表或部分。[`st.checkbox()`](/library/api-reference/widgets/st.checkbox) 接受一个参数，
在这个示例中，复选框用于切换一个条件语句，它是小部件的标签。

```python
import streamlit as st
import numpy as np
import pandas as pd

if st.checkbox('Show dataframe'):
    chart_data = pd.DataFrame(
       np.random.randn(20, 3),
       columns=['a', 'b', 'c'])

    chart_data
```

### 使用下拉框进行选项选择

使用 [`st.selectbox`](/library/api-reference/widgets/st.selectbox) 来从一系列选项中选择。您可以手动输入选项，或者通过数组或数据框的列传递选项。

让我们使用之前创建的 `df` 数据框。

```python
import streamlit as st
import pandas as pd

df = pd.DataFrame({
    'first column': [1, 2, 3, 4],
    'second column': [10, 20, 30, 40]
    })

option = st.selectbox(
    'Which number do you like best?',
     df['first column'])

'You selected: ', option
```

## 布局

Streamlit使得在左侧面板边栏中组织小部件变得非常容易，可以使用[`st.sidebar`](/library/api-reference/layout/st.sidebar)实现。每个传递给[`st.sidebar`](/library/api-reference/layout/st.sidebar)的元素都会固定在左侧，这样用户就可以专注于应用程序中的内容，同时仍然可以访问UI控件。

例如，如果您想要将一个选择框和一个滑块添加到侧边栏中，
使用 `st.sidebar.slider` 和 `st.sidebar.selectbox` 替代 `st.slider` 和 `st.selectbox`：

```python
import streamlit as st

# Add a selectbox to the sidebar:
add_selectbox = st.sidebar.selectbox(
    'How would you like to be contacted?',
    ('Email', 'Home phone', 'Mobile phone')
)

# Add a slider to the sidebar:
add_slider = st.sidebar.slider(
    'Select a range of values',
    0.0, 100.0, (25.0, 75.0)
)
```

在侧边栏之外，Streamlit还提供了其他几种控制应用程序布局的方式。[`st.columns`](/library/api-reference/layout/st.columns)允许您将小部件并排放置，而[`st.expander`](/library/api-reference/layout/st.expander)则可以通过隐藏大型内容来节省空间。

```python
import streamlit as st

left_column, right_column = st.columns(2)
# You can use a column just like st.sidebar:
left_column.button('Press me!')

# Or even better, call Streamlit functions inside a "with" block:
with right_column:
    chosen = st.radio(
        'Sorting hat',
        ("Gryffindor", "Ravenclaw", "Hufflepuff", "Slytherin"))
    st.write(f"You are in {chosen} house!")
```

<注意>

`st.echo`和`st.spinner`目前不支持在侧边栏或布局选项中使用。但请放心，我们正在努力添加对它们的支持！

</注意>

### 显示进度

当向应用程序添加长时间运行的计算时，您可以使用[`st.progress()`](/library/api-reference/status/st.progress)实时显示状态。

首先，让我们导入time模块。我们将使用`time.sleep()`方法来模拟长时间运行的计算：

```python
import time
```

现在，让我们创建一个进度条：

```python
import streamlit as st
import time

'Starting a long computation...'

# Add a placeholder
latest_iteration = st.empty()
bar = st.progress(0)

for i in range(100):
  # Update the progress bar with each iteration.
  latest_iteration.text(f'Iteration {i+1}')
  bar.progress(i + 1)
  time.sleep(0.1)

'...and now we\'re done!'
```

## 主题

Streamlit支持开箱即用的浅色和深色主题。Streamlit首先会检查查看应用程序的用户是否已经在操作系统和浏览器中设置了浅色或深色模式偏好。如果是这样，那么将使用该偏好设置。否则，默认应用浅色主题。

您也可以从"☰" → "设置"中更改活动主题。

![更改主题](/images/change_theme.gif)

想要向应用程序添加自定义主题吗？"设置"菜单中有一个主题编辑器。
单击“编辑活动主题”即可访问。您可以使用此编辑器尝试不同的颜色，并实时查看您的应用程序更新。

![编辑主题](/images/edit_theme.gif)

当您对您的工作满意时，可以通过在`[theme]`配置部分中[设置配置选项](/library/advanced-features/configuration#set-configuration-options)来保存主题。在为应用程序定义主题后，它将在主题选择器中显示为“自定义主题”并应用该主题。
在定义主题时，默认使用的是默认主题，而不是包含的亮色和暗色主题。

有关定义主题时可用选项的更多信息，请参阅[主题选项文档](/library/advanced-features/theming)。

<Note>

主题编辑器菜单仅在本地开发中可用。如果您使用Streamlit Community Cloud部署了您的应用程序，则在“设置”菜单中将不再显示“编辑当前主题”按钮。

</Note>

<Tip>

另一种尝试不同主题颜色的方法是打开"在保存时运行"选项，编辑您的config.toml文件，并观察应用程序使用新的主题颜色重新运行。

缓存

Streamlit缓存允许您的应用在从网络加载数据、操作大型数据集或执行昂贵的计算时保持高性能。

缓存的基本思想是存储昂贵函数调用的结果，并在再次出现相同输入时返回缓存的结果，而不是在后续运行中调用该函数。

要在Streamlit中对函数进行缓存，您需要使用两个装饰器之一（`st.cache_data`和`st.cache_resource`）对其进行修饰：

```python
@st.cache_data
def long_running_function(param1, param2):
    return …
```

在这个例子中，使用`@st.cache_data`装饰`long_running_function`告诉Streamlit，每当调用该函数时，它会检查两件事情：

1. 输入参数的值（在这种情况下是`param1`和`param2`）。
2. 函数内部的代码。

如果这是Streamlit第一次看到这些参数值和函数代码，它会运行函数并将返回值存储在缓存中。下次调用该函数时，只要参数和代码相同（例如，当用户与应用程序交互时），Streamlit将跳过执行函数，直接返回缓存的值。在开发过程中，随着函数代码的更改，缓存会自动更新，以确保缓存中反映了最新的更改。

如前所述，有两种缓存装饰器：

- `st.cache_data` 是推荐的缓存计算结果的方式：可以从CSV加载DataFrame，转换NumPy数组，查询API或者任何返回可序列化数据对象（如str、int、float、DataFrame、array、list等）的函数。它在每次函数调用时创建数据的新副本，从而可以安全地抵御[变异和竞争条件](/library/advanced-features/caching#mutation-and-concurrency-issues)。在大多数情况下，`st.cache_data` 的行为是您所期望的 - 因此，如果您不确定，请先尝试使用 `st.cache_data`，看看是否可以正常工作！
- `st.cache_resource` 是推荐的方式来缓存全局资源，如 ML 模型或数据库连接等无法序列化的对象，您不希望多次加载这些对象。使用它，您可以在应用程序的所有重新运行和会话中共享这些资源，而无需复制或重复。请注意，对缓存返回值的任何更改都会直接修改缓存中的对象（更多详细信息见下文）。

![Streamlit的两个缓存修饰器及其用途。](/images/caching-high-level-diagram.png "Streamlit的两个缓存修饰器及其用途。使用st.cache_data来存储数据库中的任何内容。使用st.cache_resource来存储无法存储在数据库中的任何内容，例如与数据库的连接或机器学习模型。")

有关Streamlit缓存装饰器的更多信息，包括其配置参数和限制，请参阅[Caching](/library/advanced-features/caching)。

## 页面

随着应用程序的规模增大，将其组织为多个页面变得很有用。这样可以使开发人员更容易管理应用程序，用户更容易导航。Streamlit提供了一种无缝创建多页应用程序的方式。

我们设计了这个功能，使得构建多页应用程序就像构建单页应用程序一样简单！只需要按照以下步骤将更多页面添加到现有应用中：

1. 在包含主要脚本的文件夹中，创建一个新的`pages`文件夹。假设你的主要脚本名为`main_page.py`。
2. 在`pages`文件夹中添加新的`.py`文件以添加更多页面到你的应用程序中。
3. 像往常一样运行`streamlit run main_page.py`。

就是这样！`main_page.py`脚本现在将对应于您的应用程序的主页。您将在侧边栏页面选择器中看到来自`pages`文件夹的其他脚本。例如：

<details open>
<summary><code>main_page.py</code></summary>

```python
import streamlit as st

st.markdown("# Main page 🎈")
st.sidebar.markdown("# Main page 🎈")
```

</details>

<details open>
<summary><code>pages/page_2.py</code></summary>

```python
import streamlit as st

st.markdown("# Page 2 ❄️")
st.sidebar.markdown("# Page 2 ❄️")
```

</details>

<details open>
<summary><code>pages/page_3.py</code></summary>

```python
import streamlit as st

st.markdown("# Page 3 🎉")
st.sidebar.markdown("# Page 3 🎉")
```

</details>
<br />

现在运行 `streamlit run main_page.py` 并查看您全新的多页面应用程序！

<Image src="/images/mpa-main-concepts.gif" />

我们的 [多页面应用程序文档](/library/get-started/multipage-apps) 教您如何向应用程序中添加页面，包括如何定义页面、组织和运行多页面应用程序以及在页面之间导航。一旦您掌握了基础知识，就可以 [创建您的第一个多页面应用程序](/library/get-started/multipage-apps/create-a-multipage-app)！

## 应用程序模型

现在你对所有的组成部分有了一些了解，让我们来总结一下它们是如何协同工作的：

1. Streamlit 应用程序是从上到下运行的 Python 脚本
2. 每当用户打开指向您的应用程序的浏览器选项卡时，脚本会重新执行
3. 脚本执行时，Streamlit 会即时在浏览器中展示输出
4. 脚本使用 Streamlit 缓存来避免重新计算昂贵的函数，因此更新非常快速
1. 每当用户与小部件进行交互时，您的脚本会重新执行，并且该小部件的输出值在此运行期间设置为新值。
2. Streamlit应用程序可以包含多个页面，这些页面在`pages`文件夹中的单独的`.py`文件中定义。

![Streamlit应用程序模型](/images/app_model.png)