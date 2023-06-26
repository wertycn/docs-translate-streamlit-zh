---
slug: /library/get-started/create-an-app
title: Create an app
---

# 创建一个应用程序

如果你已经完成了上述步骤，那么很可能你已经[安装了Streamlit](/library/get-started/installation)并且在我们的[主要概念](/library/get-started/main-concepts)指南中了解了一些基础知识。如果还没有这么做，现在是个好时机去看一看。

学习如何使用Streamlit的最简单方法就是自己尝试。在阅读本指南的同时，测试每个方法。只要您的应用程序正在运行，每次在脚本中添加新元素并保存时，Streamlit的UI都会询问您是否要重新运行应用程序并查看更改。这使您可以在快速的交互循环中工作：您编写一些代码，保存它，查看输出，再编写一些代码，依此类推，直到对结果满意为止。我们的目标是使用Streamlit为您的数据或模型创建一个交互式应用程序，并在此过程中使用Streamlit来审查、调试、优化和共享您的代码。

在本指南中，您将使用Streamlit的核心功能创建一个交互式应用程序；探索纽约市的公共Uber数据集中的上车和下车情况。完成后，您将了解如何获取和缓存数据，绘制图表，将信息绘制在地图上，并使用交互式小部件（如滑块）来过滤结果。

<提示>

如果您想跳过并一次性查看所有内容，可以在下方找到[完整的脚本](#lets-put-it-all-together)。

</提示>

## 创建你的第一个应用程序

Streamlit不仅仅是一个创建数据应用程序的方式，它还是一个由创作者组成的社区，他们分享他们的应用和想法，并互相帮助改进他们的工作。请加入我们的社区论坛，我们非常乐意听到你的问题、想法，并帮助你解决bug——立即加入吧！

1. 第一步是创建一个新的Python脚本。我们将其称为`uber_pickups.py`。

2. 在你喜欢的集成开发环境（IDE）或文本编辑器中打开`uber_pickups.py`，然后添加以下内容
   代码行:

   ```python
   import streamlit as st
   import pandas as pd
   import numpy as np
   ```

3. 每个好的应用程序都有一个标题，所以让我们添加一个:

   ```python
   st.title('纽约市的Uber接送')
   ```

4. 现在是时候从命令行运行Streamlit了:

   ```bash
   streamlit run uber_pickups.py
   ```

   运行Streamlit应用程序与运行其他Python脚本没有任何区别。每当您需要查看应用程序时，可以使用此命令。

    <Tip>

   您知道您还可以将URL传递给 `streamlit run` 吗？这在与GitHub Gists结合使用时非常方便。例如：

   ```bash
   streamlit run https://raw.githubusercontent.com/streamlit/demo-uber-nyc-pickups/master/streamlit_app.py
   ```

   </提示>

5. 通常情况下，应用程序应该会自动在您的浏览器中的一个新标签页中打开。

## 获取一些数据

现在您已经有了一个应用程序，下一步需要做的是获取纽约市乘客上下车的Uber数据集。

1. 让我们从编写一个加载数据的函数开始。将以下代码添加到您的脚本中：

   ```python
   DATE_COLUMN = 'date/time'
   DATA_URL = ('https://s3-us-west-2.amazonaws.com/'
            'streamlit-demo-data/uber-raw-data-sep14.csv.gz')

   def load_data(nrows):
       data = pd.read_csv(DATA_URL, nrows=nrows)
       lowercase = lambda x: str(x).lower()
       data.rename(lowercase, axis='columns', inplace=True)
       data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
       返回数据
   ```

   你会注意到`load_data`是一个普通的函数，它下载一些数据，将其放入Pandas dataframe，并将日期列从文本转换为日期时间。该函数接受一个参数(`nrows`)，用于指定要加载到dataframe中的行数。

2. 现在让我们测试这个函数并查看输出。在你的函数下面，添加以下代码行：

   ```python
   # 创建一个文本元素，告诉读者数据正在加载中。
   data_load_state = st.text('正在加载数据...')
   # 将10000行数据加载到数据框中。
   data = load_data(10000)
   # 通知读者数据已成功加载。
   data_load_state.text('加载数据完成！')
   ```

   您将在应用程序右上角看到几个按钮，询问您是否要重新运行应用程序。选择**始终重新运行**，然后您将看到您的
   每次保存时，它会自动更改。

好吧，这不那么令人激动...

事实证明，下载数据并将10,000行加载到数据帧中需要很长时间。将日期列转换为日期时间也不是一件快速的工作。您不希望每次更新应用程序时都重新加载数据-幸运的是，Streamlit允许您缓存数据。

## 无需努力的缓存

1. 尝试在`load_data`声明之前添加`@st.cache_data`：

   ```python
   @st.cache_data
   def load_data(nrows):
   ```

2. 然后保存脚本，Streamlit将自动重新运行您的应用程序。由于这是您第一次使用`@st.cache_data`运行脚本，您不会看到任何变化。让我们再调整一下文件，以便您可以看到缓存的威力。

3. 将`data_load_state.text('Loading data...done!')`这一行替换为以下内容：

   ```python
   data_load_state.text("完成！（使用st.cache_data）")
   ```

4. 现在保存。看看你添加的那一行是不是立刻出现了？如果你稍微退后一步，这实际上是非常令人惊讶的。在幕后发生了一些神奇的事情，只需要一行代码就可以激活它。

### 它是如何工作的？

让我们花几分钟来讨论`@st.cache_data`是如何工作的。

当你使用Streamlit的缓存注释标记一个函数时，它告诉Streamlit，每当该函数被调用时，它应该检查两件事情：

1. 您在函数调用中使用的输入参数。
2. 函数内的代码。

如果这是Streamlit第一次看到这两个项，并且具有这些确切的值和确切的组合，它会运行函数并将结果存储在本地缓存中。下次调用函数时，如果这两个值没有更改，Streamlit就知道它可以完全跳过执行函数。相反，它从本地缓存中读取输出并将其传递给下一个步骤。
对于调用者来说，就像魔术一样。

“但是，等一下，”你会对自己说，“这听起来太好了，不可能吧。这一切的限制是什么？”

嗯，有几个限制：

1. Streamlit只会检查当前工作目录中的更改。如果你升级了一个Python库，只有在该库安装在你的工作目录内时，Streamlit的缓存才会注意到这一点。
2. 如果你的函数不确定性（即，其输出依赖于随机因素），那么每次调用函数时，Streamlit都会重新计算函数的输出。
   1. 如果函数的输出结果依赖于不可变的输入参数（例如数字），或者它从外部的时变数据源中获取数据（例如实时股票市场行情服务），缓存的值将不会受到影响。
2. 最后，应避免修改使用 `st.cache_data` 缓存的函数的输出结果，因为缓存的值是按引用存储的。

尽管需要牢记这些限制，但在很多情况下它们并不是问题。在这些情况下，缓存确实起到了非常重要的作用。

<提示>

每当您的代码中有一个长时间运行的计算任务时，考虑重构代码以便能够使用`@st.cache_data`进行缓存，如果可能的话。请阅读[Caching](/library/advanced-features/caching)以获取更多详细信息。
</Tip>

现在您已经了解了如何使用Streamlit进行缓存，让我们回到Uber接送数据。

## 检查原始数据

在开始处理数据之前，查看一下您要处理的原始数据总是一个好主意。让我们添加一个子标题和一个打印输出:
将原始数据传输到应用程序中：

```python
st.subheader('Raw data')
st.write(data)
```

在[主要概念](/library/get-started/main-concepts)指南中，您学到了[`st.write`](/library/api-reference/write-magic/st.write)可以渲染您传递给它的几乎任何内容。在这种情况下，您传递了一个数据框，并且它以交互表格的形式呈现。

[`st.write`](/library/api-reference/write-magic/st.write)根据输入的数据类型尝试做正确的事情。如果它没有按照您的期望执行操作，您可以使用
使用[`st.dataframe`](/library/api-reference/data/st.dataframe)等专用命令。要获取完整列表，请参见[API参考](/library/api-reference)。

## 绘制直方图

现在，您已经有机会查看数据集并观察可用的内容，让我们再进一步绘制一个直方图，以查看Uber在纽约市最繁忙的小时。

1. 首先，在原始数据部分下方添加一个副标题：

   ```python
   st.subheader('按小时计算的乘车次数')

```
2. 使用NumPy生成一个直方图，按小时对乘车时间进行分组：

```python
hist_values = np.histogram(
   data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]
```

3. 现在，让我们使用Streamlit的[`st.bar_chart()`](/library/api-reference/charts/st.bar_chart)方法来绘制这个直方图。

```python
st.bar_chart(hist_values)
```

4. 保存您的脚本。这个直方图应该立即在您的应用程序中显示出来。
   快速查看后，我们发现最繁忙的时间是17:00（下午5点）。

为了绘制这个图表，我们使用了Streamlit的原生`bar_chart()`方法，但是值得注意的是，Streamlit还支持更复杂的图表库，如Altair、Bokeh、Plotly、Matplotlib等。有关完整列表，请参阅[supported charting libraries](/library/api-reference/charts)。

## 在地图上绘制数据

使用Uber的数据集和直方图可以帮助我们确定乘客叫车的最繁忙时间，但是如果我们想弄清楚乘客叫车在整个城市中的分布情况怎么办？虽然您可以使用柱状图来展示这些数据，但是除非您对城市的纬度和经度坐标非常熟悉，否则很难进行解读。为了展示乘客叫车的分布情况，让我们使用Streamlit的`st.map()`函数。
在纽约市的地图上叠加数据的函数。

1. 为这个部分添加一个子标题:

   ```python
   st.subheader('所有乘客上车点的地图')
   ```

2. 使用 `st.map()` 函数来绘制数据:

   ```python
   st.map(data)
   ```

3. 保存你的脚本。这个地图是完全交互的。尝试一下通过平移或缩放来操作。

在绘制直方图之后，你确定Uber乘车最繁忙的时间是17:00。让我们重新绘制地图，显示乘车点的集中情况。
在17:00时。

1. 定位以下代码片段：

   ```python
   st.subheader('所有接送点的地图')
   st.map(data)
   ```

2. 将其替换为：

   ```python
   hour_to_filter = 17
   filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]
   st.subheader(f'在{hour_to_filter}:00的所有接送点的地图')
   st.map(filtered_data)
   ```

3. 您应该立即看到数据更新。

为了绘制这张地图，我们使用了内置在Streamlit中的`st.map`函数，但是如果您想要可视化复杂的地图数据，我们鼓励您查看`st.pydeck_chart`函数。

## 使用滑块筛选结果

在上一节中，当您绘制地图时，用于筛选结果的时间是硬编码在脚本中的，但是如果我们希望让读者动态地筛选结果呢？
实时过滤数据？使用Streamlit的小部件可以实现。让我们使用`st.slider()`方法向应用程序添加一个滑块。

1. 找到`hour_to_filter`并将其替换为以下代码片段：

   ```python
   hour_to_filter = st.slider('小时', 0, 23, 17)  # 最小值：0小时，最大值：23小时，默认值：17小时
   ```

2. 使用滑块并观察地图实时更新。

## 使用按钮切换数据

滑块只是动态更改应用程序组成的一种方式。
让我们使用[`st.checkbox`](/library/api-reference/widgets/st.checkbox)函数向您的应用程序添加一个复选框。我们将使用这个复选框来显示/隐藏您应用程序顶部的原始数据表格。

1. 找到以下代码行：

   ```python
   st.subheader('原始数据')
   st.write(data)
   ```

2. 将这些行替换为以下代码：

   ```python
   if st.checkbox('显示原始数据'):
       st.subheader('原始数据')
       st.write(data)
   ```

我们相信您有自己的想法。完成本教程后，可以查看Streamlit在我们的[API参考](/library/api-reference)中提供的所有小部件。

## 让我们把它们整合起来

就是这样，您已经完成了。这是我们交互式应用程序的完整脚本。

<Tip>

如果您跳过了前面的内容，在创建脚本后，运行Streamlit的命令是 `streamlit run [应用程序名称]`。

</Tip>

```python
import streamlit as st
import pandas as pd
import numpy as np

st.title('Uber pickups in NYC')

DATE_COLUMN = 'date/time'
DATA_URL = ('https://s3-us-west-2.amazonaws.com/'
            'streamlit-demo-data/uber-raw-data-sep14.csv.gz')

@st.cache_data
def load_data(nrows):
    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis='columns', inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

data_load_state = st.text('Loading data...')
data = load_data(10000)
data_load_state.text("Done! (using st.cache_data)")

if st.checkbox('Show raw data'):
    st.subheader('Raw data')
    st.write(data)

st.subheader('Number of pickups by hour')
hist_values = np.histogram(data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]
st.bar_chart(hist_values)

# Some number in the range 0-23
hour_to_filter = st.slider('hour', 0, 23, 17)
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]

st.subheader('Map of all pickups at %s:00' % hour_to_filter)
st.map(filtered_data)
```

## 分享你的应用程序

在构建完Streamlit应用程序之后，是时候分享它了！为了向世界展示你的应用程序，你可以使用**Streamlit社区云**来免费部署、管理和分享你的应用程序。

它的工作原理很简单，分为以下3个步骤：

1. 将你的应用程序放在一个公共的GitHub仓库中（确保它包含一个requirements.txt文件！）
2. 登录[share.streamlit.io](https://share.streamlit.io)
3. 点击“部署应用程序”，然后粘贴你的GitHub URL

就是这样！🎈您现在拥有一个可以与全世界分享的公开部署的应用程序。点击了解更多关于[如何使用Streamlit社区云](/streamlit-community-cloud)的信息。

## 获取帮助

这就是入门的全部内容，现在您可以开始构建自己的应用程序了！如果您遇到困难，可以尝试以下几个方法：

- 查看我们的[社区论坛](https://discuss.streamlit.io/)并发布问题
- 使用命令行快速获取帮助，输入`streamlit help`
- 查看我们的[知识库](/knowledge-base)，获取有关创建和部署Streamlit应用程序的提示、逐步教程和文章，解答您的问题。
- 阅读更多文档！请查看：
  - [高级功能](/library/advanced-features)，了解如何使用缓存、主题和添加应用程序状态等高级功能。
  - [API参考](/library/api-reference/)，提供每个Streamlit命令的示例。