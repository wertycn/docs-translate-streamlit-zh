---
标题：缓存
路径：/library/advanced-features/caching

<Note>

已弃用的`@st.cache`装饰器的文档可以在[使用st.cache优化性能](/library/advanced-features/st.cache)中找到。

</Note>

# 缓存

Streamlit在每次用户交互或代码更改时从上到下运行脚本。这种执行模式使得开发非常容易。但它也带来了两个主要的挑战：

1. 长时间运行的函数会一遍又一遍地运行，这会减慢您的应用程序的运行速度。
2. 对象会被反复创建，这使得在重新运行或会话之间持久化它们变得困难。

但是不用担心！Streamlit通过其内置的缓存机制让您解决这两个问题。缓存机制存储了慢速函数调用的结果，因此它们只需运行一次。这使得您的应用程序运行速度更快，并且有助于在重新运行时持久化对象。

<折叠标题="目录" 打开={true}>

1. [最小示例](#minimal-example)
2. [基本用法](#basic-usage)
3. [高级用法](#advanced-usage)
4. [从st.cache迁移](#migrating-from-stcache)

</Collapse>

## 最小示例

要在Streamlit中对函数进行缓存，您必须使用两个装饰器之一（`st.cache_data`或`st.cache_resource`）进行修饰：

```python
@st.cache_data
def long_running_function(param1, param2):
    return …
```

在这个示例中，使用`@st.cache_data`修饰`long_running_function`告诉Streamlit，每次调用该函数时，它会检查两件事情：

1. 输入参数的值（在这种情况下是`param1`和`param2`）。
2. 函数内部的代码。

如果这是Streamlit第一次看到这些参数值和函数代码，它会运行函数并将返回值存储在缓存中。下次调用函数时，如果参数和代码相同（例如，用户与应用程序进行交互），Streamlit将完全跳过执行函数，并返回缓存的值。在开发过程中，随着函数代码的更改，缓存会自动更新，以确保最新的更改反映在缓存中。

如上所述，有两个缓存装饰器：

- `st.cache_data`是推荐的缓存计算结果的方式：从CSV加载DataFrame、转换NumPy数组、查询API或者其他返回可序列化数据对象（如str、int、float、DataFrame、array、list等）的函数。它在每次函数调用时创建数据的新副本，可以防止[变异和竞态条件](#mutation-and-concurrency-issues)。在大多数情况下，`st.cache_data`的行为是您想要的 - 所以如果您不确定，可以先使用`st.cache_data`，看看是否可行！
- `st.cache_resource`是推荐的一种方式，用于缓存全局资源，例如ML模型或数据库连接 - 这些无法序列化的对象，您不希望加载多次。使用它，您可以在应用程序的所有重新运行和会话中共享这些资源，而无需复制或重复。请注意，对缓存的返回值进行的任何修改都直接修改了缓存中的对象（下面有更多详细信息）。

![Streamlit的两个缓存修饰符及其用例。](/images/caching-high-level-diagram.png)

## 基本用法

### st.cache_data

`st.cache_data` 是用于返回数据的所有函数的命令 - 无论是 DataFrame、NumPy 数组、str、int、float 还是其他可序列化类型。对于几乎所有的用例，这都是正确的命令！

#### 用法

<br />

让我们来看一个使用 `st.cache_data` 的示例。假设您的应用程序从互联网加载了一个 Uber 乘车共享数据集 - 一个大小为 50 MB 的 CSV 文件，并将其加载到一个 DataFrame 中：

```python
def load_data(url):
    df = pd.read_csv(url)  # 👈 Download the data
    return df

df = load_data("https://github.com/plotly/datasets/raw/master/uber-rides-data1.csv")
st.dataframe(df)

st.button("Rerun")
```

运行`load_data`函数需要2到30秒，具体时间取决于您的网络连接速度。（提示：如果您的网络连接较慢，可以使用[这个大小为5MB的数据集](https://github.com/plotly/datasets/blob/master/26k-consumer-complaints.csv)）。如果没有进行缓存，每次加载应用程序或进行用户交互时都会重新下载数据。您可以通过点击我们添加的按钮自行尝试！这并不是一个很好的体验... 😕

现在让我们在`load_data`函数上添加`@st.cache_data`装饰器：

```python
@st.cache_data  # 👈 Add the caching decorator
def load_data(url):
    df = pd.read_csv(url)
    return df

df = load_data("https://github.com/plotly/datasets/raw/master/uber-rides-data1.csv")
st.dataframe(df)

st.button("Rerun")
```

再次运行应用程序。您会注意到慢速下载只会在第一次运行时发生。每次重新运行应该几乎是即时的！ 💨

#### 行为

<br />

这是如何工作的？让我们逐步了解`st.cache_data`的行为：

- 在第一次运行时，Streamlit会识别到它从未使用指定的参数值（CSV文件的URL）调用过`load_data`函数。因此，它会运行该函数并下载数据。
- 现在我们的缓存机制变得活跃起来：返回的DataFrame通过[pickle](https://docs.python.org/3/library/pickle.html)进行序列化（转换为字节），并与`url`参数的值一起存储在缓存中。
- 在下一次运行时，Streamlit会检查缓存中是否存在具有特定`url`的`load_data`条目。存在一个！因此，它会检索缓存的对象，将其反序列化为DataFrame，并返回它，而不是重新运行函数并重新下载数据。

将缓存对象进行序列化和反序列化的过程会创建我们原始DataFrame的副本。虽然这种复制行为可能看起来多余，但在缓存数据对象时，这正是我们所希望的，因为它可以有效地防止突变和并发问题。请阅读下面的“[突变和并发问题](#mutation-and-concurrency-issues)”一节，以更详细地了解这一点。

#### 示例

<br/>

**DataFrame转换**

在上面的示例中，我们已经展示了如何缓存加载DataFrame。对于`df.filter`、`df.apply`或`df.sort_values`等DataFrame转换也可以使用缓存。特别是对于大型DataFrame，这些操作可能会很慢。

```python
@st.cache_data
def transform(df):
    df = df.filter(items=['one', 'three'])
    df = df.apply(np.sum, axis=0)
	return df
```

**数组计算**

类似地，对于NumPy数组，缓存计算也是有意义的：

```python
@st.cache_data
def add(arr1, arr2):
	return arr1 + arr2
```

**数据库查询**

在使用数据库时，通常需要进行SQL查询来加载数据到您的应用程序中。反复运行这些查询可能会很慢，耗费金钱，并降低数据库的性能。我们强烈建议在您的应用程序中对任何数据库查询进行缓存。有关详细示例，请参阅[我们关于将Streamlit连接到不同数据库的指南](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources)。

```python
connection = database.connect()

@st.cache_data
def query():
    return pd.read_sql_query("SELECT * from table", connection)
```

<Tip>

你应该设置一个 `ttl`（存活时间）来从数据库获取新的结果。如果你设置 `st.cache_data(ttl=3600)` ，Streamlit 将在 1 小时（3600 秒）后使任何缓存的值无效，并重新运行缓存的函数。详见[控制缓存大小和持续时间](#控制缓存大小和持续时间)。
</Tip>

**API 调用**

同样，对 API 调用进行缓存也是有意义的。这样做还可以避免速率限制。

```python
@st.cache_data
def api_call():
    response = requests.get('https://jsonplaceholder.typicode.com/posts/1')
    return response.json()
```

**运行ML模型（推理）**

运行复杂的机器学习模型可能会消耗大量的时间和内存。为了避免重复运行相同的计算，可以使用缓存。

```python
@st.cache_data
def run_model(inputs):
    return model(inputs)
```

### st.cache_resource

`st.cache_resource` 是一个用来缓存“资源”的命令，这些资源应该在所有用户、会话和重新运行中都可用。它的使用情况比 `st.cache_data` 更有限，特别是用于缓存数据库连接和机器学习模型。

#### 使用方法

作为对`st.cache_resource`的示例，让我们来看一个典型的机器学习应用程序。首先，我们需要加载一个机器学习模型。我们使用[Hugging Face的transformers库](https://huggingface.co/docs/transformers/index)来完成这个步骤：

```python
from transformers import pipeline
model = pipeline("sentiment-analysis")  # 👈 Load the model
```

如果我们直接将这段代码放入Streamlit应用程序中，每次重新运行或用户交互时，应用程序都会重新加载模型。反复加载模型会带来两个问题：

- 加载模型需要时间，会拖慢应用程序的速度。
- 每个会话都会从头开始加载模型，这会占用大量的内存空间。

相反，更有意义的做法是加载模型一次，然后在所有用户和会话中都使用同一个对象。这正是`st.cache_resource`的用例！让我们将其添加到我们的应用程序中，并处理一些用户输入的文本：

```python
from transformers import pipeline

@st.cache_resource  # 👈 Add the caching decorator
def load_model():
    return pipeline("sentiment-analysis")

model = load_model()

query = st.text_input("Your query", value="I love Streamlit! 🎈")
if query:
    result = model(query)[0]  # 👈 Classify the query text
    st.write(result)
```

如果您运行此应用程序，您会发现应用程序只在启动时调用`load_model`一次。后续的运行将重用存储在缓存中的同一模型，节省时间和内存！

#### 行为

<br />

使用`st.cache_resource`与使用`st.cache_data`非常相似。但是在行为上有一些重要的区别:

- `st.cache_resource`不会创建缓存返回值的副本，而是直接将对象存储在缓存中。对函数返回值的所有更改都会直接影响缓存中的对象，因此您必须确保来自多个会话的更改不会引起问题。简而言之，返回值必须是线程安全的。

    <警告>

  在不支持线程安全的对象上使用`st.cache_resource`可能会导致崩溃或数据损坏。请在下面的[变异和并发问题](#mutation-and-concurrency-issues)中了解更多信息。
</Warning>

- 不创建副本意味着缓存的返回对象只有一个全局实例，这样可以节省内存，例如在使用大型机器学习模型时。从计算机科学的角度来说，我们创建了一个[单例模式](https://en.wikipedia.org/wiki/Singleton_pattern)。
- 函数的返回值不需要是可序列化的。这种行为非常适合那些本质上不可序列化的类型，例如数据库连接、文件句柄或线程。无法使用`st.cache_data`对这些对象进行缓存。

#### 示例

<br />

**数据库连接**

`st.cache_resource`用于连接数据库。通常情况下，您会创建一个连接对象，希望在每次查询中都可以重用它。每次运行都创建一个新的连接对象将是低效的，并且可能导致连接错误。这正是`st.cache_resource`可以做到的，例如对于一个Postgres数据库：

```python
@st.cache_resource
def init_connection():
    host = "hh-pgsql-public.ebi.ac.uk"
    database = "pfmegrnargs"
    user = "reader"
    password = "NWDMCE5xdipIjRrp"
    return psycopg2.connect(host=host, database=database, user=user, password=password)

conn = init_connection()
```

当然，您也可以对其他任何数据库执行相同的操作。请查看[我们关于如何将Streamlit连接到数据库的指南](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources)以获取更详细的示例。

**加载机器学习模型**

您的应用程序应始终缓存ML模型，以便它们不会在每个新会话中重新加载到内存中。有关如何使用🤗 Hugging Face模型进行缓存的示例，请参见上面的[示例](#usage-1)。您也可以对PyTorch、TensorFlow等进行相同的操作。以下是PyTorch的一个示例：

```python
@st.cache_resource
def load_model():
    model = torchvision.models.resnet50(weights=ResNet50_Weights.DEFAULT)
    model.eval()
    return model

model = load_model()
```

### 决定使用哪个缓存装饰器

<br />

上面的章节展示了每个缓存装饰器的许多常见示例。但是对于一些边缘情况，决定使用哪个缓存装饰器可能不那么容易。最终，这取决于“数据”和“资源”的区别。

- 数据是可序列化的对象（可以通过[pickle](https://docs.python.org/3/library/pickle.html)将其转换为字节），您可以轻松地保存到磁盘上。想象一下，您通常会存储在数据库或文件系统中的所有类型 - 基本类型如str，int和float，还有数组，DataFrames，图像或这些类型的组合（列表，元组，字典等）。
- 资源是通常不会保存到磁盘或数据库的不可序列化对象。它们通常是更复杂、非永久性的对象，如数据库连接、机器学习模型、文件句柄、线程等。

从上面列出的类型可以明显看出，Python 中的大多数对象都是“数据”。这也是为什么在几乎所有情况下，`st.cache_data` 是正确的命令。`st.cache_resource` 是一个更特殊的命令，只应在特定情况下使用。

或者如果您懒得思考或者不想花太多时间，可以在下面的表格中查找您的用例或返回类型 😉：

| 用例                               |                                                                                                    典型的返回类型 |                                                                                                                                            缓存装饰器 |
| :----------------------------------- | -------------------------------------------------------------------------------------------------------------------------: | -----------------------------------------------------------------------------------------------------------------------------------------------------------: |
| 使用pd.read_csv读取CSV文件 | pandas.DataFrame | st.cache_data |
| 读取文本文件 | str, str列表 | st.cache_data |
| 转换pandas数据框 | pandas.DataFrame，pandas.Series | st.cache_data |
| 使用NumPy数组进行计算 | numpy.ndarray |
| :------------------------ | :----------- |
| st.cache_data | 未找到内容 |
| 使用基本类型进行简单计算 |                                                                                                         str, int, float, … |                                                                                                                                                st.cache_data |
| 查询数据库                      | pandas.DataFrame | st.cache_data |
| 查询 API | pandas.DataFrame，str，dict | st.cache_data |
| 运行机器学习模型（推断） | pandas.DataFrame, str, int, dict, list | st.cache_data |
| 创建或处理图像 | PIL.Image.Image, numpy.ndarray | st.cache_data |
| 创建图表 | matplotlib.figure.Figure，plotly.graph_objects.Figure，altair.Chart | st.cache_data（但是某些库需要st.cache_resource，因为图表对象不可序列化 - 确保在创建后不要更改图表！） |
| 加载机器学习模型 | transformers.Pipeline, torch.nn.Module, tensorflow.keras.Model | st.cache_resource |
| 初始化数据库连接 | pyodbc.Connection，sqlalchemy.engine.base.Engine，psycopg2.connection，mysql.connector.MySQLConnection，sqlite3.Connection | st.cache_resource |
| 打开持久文件句柄 | \_io.TextIOWrapper | st.cache_resource |
| 打开持久线程           |                                                                                                           threading.thread |                                                                                                                                            st.cache_resource |

## 高级用法

### 控制缓存大小和持续时间

如果您的应用程序运行时间较长并且经常缓存函数，您可能会遇到两个问题：

1. 应用程序由于缓存过大而耗尽内存。
2. 缓存中的对象变得陈旧，例如因为您从数据库中缓存了旧数据。

您可以使用`ttl`和`max_entries`参数来解决这些问题，这两个参数都适用于缓存装饰器。

**`ttl`（生存时间）参数**

`ttl`在缓存函数上设置了一个生存时间。如果时间到期并再次调用函数，应用程序将丢弃任何旧的缓存值，并重新运行函数。然后，计算得到的新值将被存储在缓存中。这种行为对于防止过时数据（问题2）和缓存过大（问题1）很有用。特别是当从数据库或API中获取数据时，您应该始终设置一个`ttl`，以免使用旧数据。以下是一个示例：

```python
@st.cache_data(ttl=3600)  # 👈 Cache data for 1 hour (=3600 seconds)
def get_api_data():
    data = api.get(...)
    return data
```

<提示>

您也可以使用`timedelta`设置`ttl`的值，例如，`ttl=datetime.timedelta(hours=1)`。
</提示>

**`max_entries`参数**

`max_entries`用于设置缓存中的最大条目数。限制缓存条目数是有用的，可以限制内存使用（问题1），特别是在缓存大型对象时。当缓存已满时，最旧的条目将在添加新条目时被移除。下面是一个示例：

```python
@st.cache_data(max_entries=1000)  # 👈 Maximum 1000 entries in the cache
def get_large_array(seed):
    np.random.seed(seed)
    arr = np.random.rand(100000)
    return arr
```

### 自定义加载图标

默认情况下，Streamlit在应用程序中运行缓存函数时会显示一个小的加载图标。您可以通过`show_spinner`参数轻松地进行修改，该参数适用于两个缓存装饰器：

```python
@st.cache_data(show_spinner=False)  # 👈 Disable the spinner
def get_api_data():
    data = api.get(...)
    return data

@st.cache_data(show_spinner="Fetching data from API...")  # 👈 Use custom text for spinner
def get_api_data():
    data = api.get(...)
    return data
```

### 排除输入参数

在缓存函数中，所有输入参数都必须是可哈希的。让我们快速解释一下为什么以及它的意义。当函数被调用时，Streamlit会查看其参数值以确定它之前是否已被缓存过。因此，它需要一种可靠的方法来比较不同函数调用之间的参数值。对于字符串或整数来说很简单，但对于任意对象来说就比较复杂了！Streamlit使用[哈希](https://en.wikipedia.org/wiki/Hash_function)来解决这个问题。它将参数转换为一个稳定的键，并将该键存储起来。在下一次函数调用时，它会再次对参数进行哈希运算，并将其与存储的哈希键进行比较。

很遗憾，并非所有的参数都是可哈希的！例如，您可能会传递一个不可哈希的数据库连接或者机器学习模型给您的缓存函数。在这种情况下，您可以排除输入参数进行缓存。只需要在参数名前加下划线（例如，`_param1`），它将不会用于缓存。即使它发生了变化，如果所有其他参数匹配，Streamlit也会返回缓存的结果。

这里是一个例子：

```python
@st.cache_data
def fetch_data(_db_connection, num_rows):  # 👈 Don't hash _db_connection
    data = _db_connection.fetch(num_rows)
    return data

connection = init_connection()
fetch_data(connection, 10)
```

### 在缓存函数中使用Streamlit命令

#### 静态元素

从1.16.0版本开始，缓存函数可以包含Streamlit命令！例如，您可以这样做：

```python
@st.cache_data
def get_api_data():
    data = api.get(...)
    st.success("Fetched data from API!")  # 👈 Show a success message
    return data
```

正如我们所知，Streamlit只会在之前没有缓存的情况下运行这个函数。在第一次运行时，`st.success`消息将会在应用程序中显示。但是在后续的运行中会发生什么呢？它仍然会显示出来！Streamlit意识到在缓存的函数中有一个`st.`命令，它在第一次运行时进行保存，并在后续的运行中重新播放。重播静态元素对于缓存装饰器都是有效的。

您还可以使用这个功能来缓存整个UI的部分内容：

```python
@st.cache_data
def show_data():
    st.header("Data analysis")
    data = api.get(...)
    st.success("Fetched data from API!")
    st.write("Here is a plot of the data:")
    st.line_chart(data)
    st.write("And here is the raw data:")
    st.dataframe(data)
```

#### 输入小部件

您还可以在缓存函数中使用[交互式输入小部件](/library/api-reference/widgets)，例如`st.slider`或`st.text_input`。小部件回放是一个实验性功能。要启用它，您需要设置`experimental_allow_widgets`参数：

```python
@st.cache_data(experimental_allow_widgets=True)  # 👈 Set the parameter
def get_data():
    num_rows = st.slider("Number of rows to get")  # 👈 Add a slider
    data = api.get(..., num_rows)
    return data
```

Streamlit将滑块视为缓存函数的附加输入参数。如果您更改滑块位置，Streamlit将检查是否已经为该滑块值缓存了函数。如果是，则返回缓存的值。如果没有，则使用新的滑块值重新运行函数。

在缓存函数中使用小部件非常强大，因为它可以让您缓存应用程序的整个部分。但是这也可能带来一些风险！由于Streamlit将小部件值视为额外的输入参数，它可能会导致过多的内存使用。想象一下，如果您的缓存函数有五个滑动条并返回一个100 MB的DataFrame。那么我们将为这五个滑动条值的每个排列都添加100 MB到缓存中，即使这些滑动条不影响返回的数据！这些添加操作会迅速使您的缓存爆炸。如果在缓存函数中使用小部件，请注意此限制。我们建议仅在小部件直接影响缓存返回值的孤立部分使用此功能。

<警告>

对于缓存函数中的小部件的支持是实验性的。我们可能随时更改或删除它，而无需提前警告。请谨慎使用！
</警告>

<注意>

目前在缓存函数中不支持两个小部件：`st.file_uploader`和`st.camera_input`。我们可能会在将来支持它们。如果您需要这些小部件，请随时[提交一个GitHub问题](https://github.com/streamlit/streamlit/issues)！
</注意>

### 处理大数据

正如我们所解释的，您应该使用`st.cache_data`来缓存数据对象。但是对于非常大的数据，比如具有超过1亿行的DataFrames或数组，这可能会很慢。这是因为`st.cache_data`的[复制行为](#copying-behavior)：在第一次运行时，它将返回值序列化为字节并在后续运行时进行反序列化。这两个操作都需要时间。

如果您处理的是非常大的数据，使用`st.cache_resource`可能是有意义的。它不会通过序列化/反序列化创建返回值的副本，并且几乎是即时的。但是要小心：对函数返回值的任何更改（例如从DataFrame中删除列或在数组中设置值）会直接操作缓存中的对象。您必须确保这不会损坏数据或导致崩溃。请参阅下面的[变异和并发问题](#mutation-and-concurrency-issues)部分。

在对具有四列的pandas DataFrame进行`st.cache_data`基准测试时，我们发现当行数超过1亿时，它变得很慢。下表显示了不同行数下使用两种缓存装饰器的运行时间（所有行数均为四列）：

|                   |                 | 1000万行 | 5000万行 | 1亿行 | 2亿行 |
| ----------------- | --------------- | :------: | :------: | :-------: | :-------: |
| st.cache_data     | 首次运行\*     |  0.4 秒   |   3 秒    |   14 秒    |   28 秒    |
|                   | 后续运行       |  0.2 秒   |   1 秒    |    2 秒    |    7 秒    |
| st.cache_resource | 首次运行\*     |  0.01 秒  |  0.1 秒   |   0.2 秒   |    1 秒    |
|                   | 后续运行       |   0 秒    |   0 秒    |    0 秒    |    0 秒    |

|                                                                                                                                                              |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _\*对于第一次运行，表格只显示使用缓存装饰器的开销时间，不包括缓存函数本身的运行时间。_ |

### 突变和并发问题

在上面的章节中，我们详细讨论了在缓存函数的返回对象上进行突变操作时可能出现的问题。这个主题非常复杂！但它对于理解`st.cache_data`和`st.cache_resource`之间的行为差异至关重要。让我们深入一些。

首先，我们应该明确定义我们所说的突变和并发的含义：

- **突变**指的是在调用函数之后对缓存函数的返回值进行的任何更改。例如：

  ```python
  @st.cache_data
  def create_list():
      l = [1, 2, 3]

  l = create_list()  # 👈 调用函数
  l[0] = 2  # 👈 修改其返回值
  ```

- **并发性**指的是多个会话可以同时引发这些变异。Streamlit是一个需要处理许多用户和会话连接到应用程序的Web框架。如果两个人同时查看应用程序，它们都会导致Python脚本重新运行，可能同时操作缓存的返回对象 - 并发地。

修改缓存的返回对象可能是危险的。它可能会导致您的应用程序出现异常，甚至破坏您的数据（这可能比应用程序崩溃还要糟糕！）。下面，我们首先解释`st.cache_data`的复制行为，并展示它如何避免变异问题。然后，我们将展示并发变异如何导致数据损坏以及如何防止它。

#### 复制行为

`st.cache_data` 每次调用函数时都会创建缓存返回值的副本。这样可以避免大部分的突变和并发问题。要详细了解它，让我们回到上面关于 `st.cache_data` 的 [Uber ridesharing example](#usage)。我们对它进行了两个修改：

1. 我们使用 `st.cache_resource` 替代了 `st.cache_data`。`st.cache_resource` **不会**创建缓存对象的副本，这样我们可以看到没有复制行为时会发生什么。
2. 在加载数据后，我们通过删除列"Lat"来操作返回的DataFrame（原地操作）。

以下是代码示例：

```python
@st.cache_resource   # 👈 Turn off copying behavior
def load_data(url):
    df = pd.read_csv(url)
    return df

df = load_data("https://raw.githubusercontent.com/plotly/datasets/master/uber-rides-data1.csv")
st.dataframe(df)

df.drop(columns=['Lat'], inplace=True)  # 👈 Mutate the dataframe inplace

st.button("Rerun")
```

让我们运行它，看看会发生什么！第一次运行应该没问题。但在第二次运行时，你会看到一个异常：`KeyError: "['Lat'] not found in axis"`。为什么会发生这种情况？让我们一步一步来解释：

- 在第一次运行时，Streamlit运行`load_data`并将得到的DataFrame存储在缓存中。由于我们使用了`st.cache_resource`，它不会创建副本，而是存储原始的DataFrame。
- 然后我们从DataFrame中删除“Lat”列。请注意，这是从缓存中存储的_原始_ DataFrame 中删除列。我们正在对其进行操作！
- 在第二次运行时，Streamlit从缓存中返回完全相同的操作过的DataFrame。它不再有“Lat”列！因此，我们对`df.drop`的调用会导致异常。Pandas无法删除不存在的列。

`st.cache_data`的复制行为可以防止出现这种突变错误。突变只会影响特定的副本，而不会影响缓存中的底层对象。下一次重新运行将获得自己的、未经突变的DataFrame副本。您可以自己尝试一下，只需将上面的`st.cache_resource`替换为`st.cache_data`，您会发现一切都正常工作。

由于这种复制行为，建议使用`st.cache_data`来缓存数据转换和计算 - 任何返回可序列化对象的操作。

#### 并发问题

现在让我们看一下当多个用户同时修改缓存中的对象时会发生什么。假设您有一个返回列表的函数。同样，我们使用`st.cache_resource`来缓存它，以避免创建副本：

```python
@st.cache_resource
def create_list():
    l = [1, 2, 3]
    return l

l = create_list()
first_list_value = l[0]
l[0] = first_list_value + 1

st.write("l[0] is:", l[0])
```

假设用户A运行该应用程序，他们将看到以下输出：

```python
l[0] is: 2
```

假设另一个用户B在用户A之后访问该应用程序。与用户A不同，他们将看到以下输出:

```python
l[0] is: 3
```

现在，用户A在用户B之后立即重新运行应用程序。他们将看到以下输出：

```python
l[0] is: 4
```

发生了什么？为什么所有的输出都不同？

- 当用户A访问应用程序时，调用`create_list()`函数，并将列表`[1, 2, 3]`存储在缓存中。然后将此列表返回给用户A。列表的第一个值`1`被赋给`first_list_value`，并将`l[0]`改为`2`。
- 当用户B访问应用程序时，`create_list()`从缓存返回经过修改的列表：`[2, 2, 3]`。列表的第一个值`2`被赋给`first_list_value`，并将`l[0]`改为`3`。
- 当用户A重新运行应用程序时，`create_list()`再次返回变异的列表：`[3, 2, 3]`。列表的第一个值`3`被分配给`first_list_value`，并且`l[0]`被改变为4。

仔细想一想，这是有道理的。用户A和B使用同一个列表对象（存储在缓存中的对象）。由于列表对象被改变，用户A对列表对象的更改也会反映在用户B的应用程序中。

这就是为什么在同时访问应用程序的多个用户时，必须小心更改使用`st.cache_resource`缓存的对象。如果我们使用`st.cache_data`而不是`st.cache_resource`，应用程序将为每个用户复制列表对象，上述示例将按预期工作-用户A和用户B都将看到：

```python
l[0] is: 2
```

<注意>

这个玩具示例可能看起来无害。但是数据损坏可能非常危险！想象一下，如果我们在这里处理的是一个大型银行的财务记录。你肯定不想因为有人错误地使用了错误的缓存装饰器而醒来时账户里少了钱 😉

</注意>

## 从st.cache迁移

我们在Streamlit 1.18.0中引入了上述缓存命令。在此之前，我们只有一个捕获所有命令`st.cache`。使用它经常会导致困惑、异常情况和速度较慢。这就是为什么我们在1.18.0中用新的命令替换了`st.cache`（在这篇[博客文章](https://blog.streamlit.io/introducing-two-new-caching-commands-to-replace-st-cache/)中可以了解更多）。新的命令提供了一种更直观、更高效的方式来缓存您的数据和资源，并且旨在在所有新的开发中取代`st.cache`。

如果您的应用程序仍在使用`st.cache`，不要绝望！以下是一些迁移的注意事项：

- `st.cache`已被弃用。•如果您的应用程序使用它，Streamlit的新版本将显示一个弃用警告。
- 我们不会立即删除`st.cache`，所以您不需要担心您的2年前的应用程序会出现问题。但是我们鼓励您尝试新的命令 - 它们会更少烦人！
- 在大多数情况下，切换代码到新的命令应该是很容易的。要决定是使用`st.cache_data`还是`st.cache_resource`，请阅读[决定使用哪个缓存装饰器](#deciding-which-caching-decorator-to-use)。Streamlit还会识别常见的用例，并在弃用警告中显示提示信息。
- 大多数`st.cache`的参数也存在于新的命令中，只有少数例外情况：
  - `allow_output_mutation`不再存在。您可以安全地删除它。只需确保根据您的用例使用正确的缓存命令。
  - `suppress_st_warning`不再存在。您可以安全地删除它。缓存函数现在可以包含Streamlit命令并重放它们。如果您想在缓存函数中使用小部件，请将`experimental_allow_widgets=True`。请参阅[这里](#using-streamlit-commands-in-cached-functions)。
  - `hash_funcs`不再存在。您可以通过在参数前加下划线来排除参数的缓存（和哈希）：`_excluded_param`。请参阅[这里](#excluding-input-parameters)。

如果在迁移过程中有任何问题或疑问，请在[论坛](https://discuss.streamlit.io/)上联系我们，我们将很乐意帮助您。 🎈
