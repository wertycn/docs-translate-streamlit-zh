---
slug: /library/advanced-features/caching
title: Caching
---

<注意>

已弃用的`@st.cache`装饰器的文档可以在[使用st.cache优化性能](/library/advanced-features/st.cache)中找到。

</注意>

# 缓存

Streamlit在每次用户交互或代码更改时从上到下运行您的脚本。这种执行模型使开发变得非常简单。但是它也带来了两个主要挑战：

1. 长时间运行的函数会一次又一次地运行，从而减慢应用程序的速度。
2. 对象会不断地被重新创建，这使得在重新运行或会话之间持久化它们变得困难。

但是不用担心！Streamlit提供了内置的缓存机制，可以解决这两个问题。缓存会存储慢速函数调用的结果，因此它们只需要运行一次。这使得您的应用程序运行更快，并且有助于在重新运行时持久化对象。

<折叠 标题="目录" 展开={true}>

1. [最小示例](#minimal-example)
2. [基本用法](#basic-usage)
3. [高级用法](#高级用法)
4. [从st.cache迁移](#从st.cache迁移)

</Collapse>

## 最简示例

要在Streamlit中缓存一个函数，您必须使用两个装饰器之一（`st.cache_data`或`st.cache_resource`）对其进行修饰。

```python
@st.cache_data
def long_running_function(param1, param2):
    return …
```

在这个例子中，使用`@st.cache_data`装饰`long_running_function`告诉Streamlit，每当调用该函数时，它会检查两件事情：

1. 输入参数的值（在这种情况下是`param1`和`param2`）。
2. 函数内部的代码。

如果这是Streamlit第一次看到这些参数值和函数代码，它会运行函数并将返回值存储在缓存中。下次调用函数时，如果使用相同的参数和代码（例如，当用户与应用程序交互时），Streamlit将跳过执行函数，并返回缓存的值。在开发过程中，随着函数代码的更改，缓存会自动更新，确保缓存中反映了最新的更改。

正如提到的，有两个缓存装饰器：

- `st.cache_data` 是推荐的缓存计算结果的方式：它可以缓存从CSV加载的DataFrame，转换NumPy数组，查询API或任何返回可序列化数据对象（如str，int，float，DataFrame，array，list等）的函数。每次函数调用时，它都会创建数据的新副本，因此安全防止了[变异和竞态条件](#mutation-and-concurrency-issues)。在大多数情况下，`st.cache_data` 的行为是您想要的，所以如果您不确定，可以先尝试使用 `st.cache_data` 看看是否有效！
- `st.cache_resource` 是推荐的缓存全局资源的方式，例如 ML 模型或数据库连接等不可序列化的对象，您不希望多次加载。使用它，您可以在应用的所有重新运行和会话中共享这些资源，而无需复制或重复。请注意，对缓存的返回值进行的任何修改都会直接修改缓存中的对象（更多详细信息请参见下文）。

![Streamlit的两个缓存装饰器及其用途](/images/caching-high-level-diagram.png)

## 基本用法

### st.cache_data

`st.cache_data`是您处理返回数据的函数的首选命令 - 无论是DataFrame、NumPy数组、str、int、float或其他可序列化类型。在几乎所有情况下，它都是正确的命令！

#### 使用方法

<br />

让我们来看一个使用`st.cache_data`的例子。假设您的应用程序从互联网上加载了一个50MB的[Uber共享乘车数据集](https://github.com/plotly/datasets/blob/master/uber-rides-data1.csv)，并将其存储为DataFrame：

```python
def load_data(url):
    df = pd.read_csv(url)  # 👈 Download the data
    return df

df = load_data("https://github.com/plotly/datasets/raw/master/uber-rides-data1.csv")
st.dataframe(df)

st.button("Rerun")
```

运行`load_data`函数需要2到30秒，具体时间取决于您的互联网连接速度。（提示：如果您的连接速度较慢，可以使用[这个大小为5 MB的数据集](https://github.com/plotly/datasets/blob/master/26k-consumer-complaints.csv)）。如果不使用缓存，每次加载应用程序或进行用户交互时都会重新下载数据。通过点击我们添加的按钮来自己试试吧！这并不是一个很好的体验... 😕

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

再次运行应用程序。您会注意到慢下载只会在第一次运行时发生。每次重新运行应用程序都应该几乎是瞬间完成的！ 💨

#### 行为

<br />

这是如何工作的呢？让我们逐步了解`st.cache_data`的行为:

- 在第一次运行时，Streamlit会识别到它从未使用指定的参数值（CSV文件的URL）调用过`load_data`函数。因此，它会运行该函数并下载数据。
- 现在我们的缓存机制开始生效：返回的DataFrame通过[pickle](https://docs.python.org/3/library/pickle.html)进行序列化（转换为字节），并存储在缓存中（与`url`参数的值一起）。
- 在下一次运行时，Streamlit会检查缓存中是否有一个带有特定`url`的`load_data`条目。有一个！因此，它会检索缓存对象，将其反序列化为DataFrame，并返回它，而不是重新运行函数并重新下载数据。

将缓存对象进行序列化和反序列化的过程会创建原始DataFrame的副本。尽管这种复制行为可能看起来是不必要的，但在缓存数据对象时，这正是我们所希望的，因为它有效地防止了突变和并发问题。请阅读下面的“[突变和并发问题](#mutation-and-concurrency-issues)”部分，以更详细地了解这一点。

#### 示例

<br/>

**DataFrame变换**

在上面的示例中，我们已经展示了如何缓存加载DataFrame的方法。对于DataFrame的转换操作，比如`df.filter`、`df.apply`或者`df.sort_values`，缓存也是非常有用的。特别是在处理大型DataFrame时，这些操作可能会很慢。

```python
@st.cache_data
def transform(df):
    df = df.filter(items=['one', 'three'])
    df = df.apply(np.sum, axis=0)
	return df
```

**数组计算**

类似地，缓存NumPy数组上的计算也是有意义的：

```python
@st.cache_data
def add(arr1, arr2):
	return arr1 + arr2
```

**数据库查询**

在使用数据库时，通常会使用SQL查询将数据加载到应用程序中。重复运行这些查询可能会很慢，耗费金钱，并降低数据库的性能。我们强烈建议在应用程序中缓存任何数据库查询。有关详细示例，请参阅[连接Streamlit到不同数据库的指南](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources)。

```python
connection = database.connect()

@st.cache_data
def query():
    return pd.read_sql_query("SELECT * from table", connection)
```

<提示>

您应该为数据库设置一个`ttl`（存活时间），以获取新的结果。如果您设置了`st.cache_data(ttl=3600)`，Streamlit将在1小时（3600秒）后使任何缓存的值失效，并重新运行缓存的函数。有关详细信息，请参阅[控制缓存大小和持续时间](#控制缓存大小和持续时间)。
</提示>

**API调用**

同样，缓存API调用也是有意义的。这样做还可以避免速率限制。

```python
@st.cache_data
def api_call():
    response = requests.get('https://jsonplaceholder.typicode.com/posts/1')
    return response.json()
```

**运行机器学习模型（推断）**

运行复杂的机器学习模型可能需要大量的时间和内存。为了避免重复运行相同的计算，可以使用缓存。

```python
@st.cache_data
def run_model(inputs):
    return model(inputs)
```

### st.cache_resource

`st.cache_resource` 是一个用于缓存“资源”的命令，这些资源应该在所有用户、会话和重新运行中全局可用。它的使用案例比 `st.cache_data` 更有限，特别适用于缓存数据库连接和机器学习模型。

#### 用法

作为 `st.cache_resource` 的示例，让我们来看一个典型的机器学习应用程序。作为第一步，我们需要加载一个机器学习模型。我们可以使用 [Hugging Face 的 transformers 库](https://huggingface.co/docs/transformers/index) 来实现这一点。

```python
from transformers import pipeline
model = pipeline("sentiment-analysis")  # 👈 Load the model
```

如果我们直接将这段代码放入Streamlit应用中，每次重新运行或用户交互时，应用程序都会加载模型。重复加载模型会带来两个问题：

- 加载模型需要时间，会减慢应用程序的速度。
- 每个会话都会从头开始加载模型，这会占用大量内存空间。

相反，一次加载模型，并在所有用户和会话中使用相同的对象将更加合理。这正是`st.cache_resource`的使用场景！让我们将其添加到我们的应用程序中，并处理用户输入的一些文本：

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

如果您运行此应用程序，您将看到该应用程序仅在应用程序启动时调用`load_model`一次。后续运行将重用存储在缓存中的相同模型，从而节省时间和内存！

#### 行为

<br />

使用`st.cache_resource`与使用`st.cache_data`非常相似。但是行为上有一些重要的区别：

- `st.cache_resource` 不会创建缓存返回值的副本，而是直接将对象本身存储在缓存中。对函数返回值的任何修改都会直接影响缓存中的对象，因此您必须确保来自多个会话的修改不会引起问题。简而言之，返回值必须是线程安全的。

    <警告>

  在非线程安全的对象上使用`st.cache_resource`可能会导致崩溃或数据损坏。在下面的[变异和并发问题](#mutation-and-concurrency-issues)中了解更多。

- 不创建副本意味着缓存的返回对象只有一个全局实例，这样可以节省内存，例如在使用大型机器学习模型时。从计算机科学的角度来看，我们创建了一个[单例模式](https://en.wikipedia.org/wiki/Singleton_pattern)。
- 函数的返回值不需要可序列化。这种行为非常适用于天然不可序列化的类型，比如数据库连接、文件句柄或线程。无法使用`st.cache_data`缓存这些对象。

#### 示例

<br />

**数据库连接**

`st.cache_resource`对于连接数据库非常有用。通常情况下，您会创建一个连接对象，希望在每次查询时都可以全局重用。如果每次运行都创建一个新的连接对象将会很低效，并且可能导致连接错误。这正是`st.cache_resource`可以做到的，例如对于一个Postgres数据库：

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

当然，您也可以对其他任何数据库进行相同的操作。请查看[我们关于如何将Streamlit连接到数据库的指南](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources)以获取深入的示例。

**加载机器学习模型**

您的应用程序应始终缓存机器学习模型，以便它们不会在每个新会话中再次加载到内存中。请参考上面的[示例](#usage-1)以了解如何在🤗 Hugging Face模型中实现此功能。您可以对PyTorch、TensorFlow等做同样的操作。以下是PyTorch的一个示例：

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

上面的章节展示了每个缓存装饰器的许多常见示例。但是对于一些特殊情况，决定使用哪个缓存装饰器可能并不那么简单。最终，这取决于“数据”和“资源”之间的区别：

- 数据是可序列化的对象（可以通过[pickle](https://docs.python.org/3/library/pickle.html)将其转换为字节），您可以轻松地将其保存到磁盘上。想象一下您通常会存储在数据库或文件系统中的所有类型 - 基本类型如str、int和float，以及数组、DataFrames、图像或这些类型的组合（列表、元组、字典等）。
- 资源是通常不会保存到磁盘或数据库的无法序列化的对象。它们通常是更复杂、非永久性的对象，例如数据库连接、机器学习模型、文件句柄、线程等。

从上面列出的类型可以明显看出，Python 中的大多数对象都是“数据”。这也是为什么 `st.cache_data` 是几乎所有情况下的正确命令。`st.cache_resource` 是一个更特殊的命令，您只应在特定情况下使用它。

或者如果您懒得思考或不想费太多心思，可以在下面的表格中查找您的用例或返回类型😉：

| 用例                                 |                                                                                                       典型的返回类型 |                                                                                                                                            缓存装饰器 |
| :----------------------------------- | -------------------------------------------------------------------------------------------------------------------------: | -----------------------------------------------------------------------------------------------------------------------------------------------------------: |
| 使用 pd.read_csv 读取 CSV 文件 | pandas.DataFrame | st.cache_data |
| 读取文本文件                         |                                                                                                           str, str列表 |                                                                                                                                                st.cache数据 |
| 转换pandas数据框       |                                                                                            pandas.DataFrame, pandas.Series |                                                                                                                                                st.cache_data |
| 使用numpy数组进行计算               | numpy.ndarray                                                                                           | st.cache_data |
| 使用基本类型进行简单计算 | str, int, float, ... | st.cache_data |
| 查询数据库 | pandas.DataFrame | st.cache_data |
| 查询 API                     | pandas.DataFrame，str，dict | st.cache_data |
| 运行机器学习模型（推断） | pandas.DataFrame，str，int，dict，list | st.cache_data |
| 创建或处理图像 | PIL.Image.Image, numpy.ndarray | st.cache_data |
| 创建图表 | matplotlib.figure.Figure, plotly.graph_objects.Figure, altair.Chart | st.cache_data（但是某些库需要 st.cache_resource，因为图表对象不可序列化 - 确保在创建后不要改变图表！） |
| 加载机器学习模型 | transformers.Pipeline, torch.nn.Module, tensorflow.keras.Model | st.cache_resource |
| 初始化数据库连接 | pyodbc.Connection, sqlalchemy.engine.base.Engine, psycopg2.connection, mysql.connector.MySQLConnection, sqlite3.Connection | st.cache_resource |
| 打开持久化文件句柄      |                                                                                                         \_io.TextIOWrapper |                                                                                                                                            st.cache_resource |
| 打开持久线程 | threading.thread | st.cache_resource |

## 高级用法

### 控制缓存大小和持续时间

如果您的应用程序长时间运行并且不断缓存函数，可能会遇到两个问题：

1. 应用程序由于缓存太大而耗尽内存。
2. 缓存中的对象变得陈旧，例如因为您缓存了来自数据库的旧数据。

您可以使用`ttl`和`max_entries`参数来解决这些问题，这两个参数都适用于缓存装饰器。

**`ttl`（存活时间）参数**

`ttl`在缓存函数上设置了一个存活时间。如果超过这个时间并且再次调用函数，应用程序会丢弃任何旧的缓存值，并重新运行函数。然后，计算出的新值将被存储在缓存中。这种行为对于防止数据过期（问题2）和缓存过大（问题1）非常有用。特别是在从数据库或API获取数据时，您应该始终设置一个`ttl`，以便不使用旧数据。下面是一个示例：

```python
@st.cache_data(ttl=3600)  # 👈 Cache data for 1 hour (=3600 seconds)
def get_api_data():
    data = api.get(...)
    return data
```

<Tip>

您也可以使用`timedelta`来设置`ttl`的值，例如`ttl=datetime.timedelta(hours=1)`。
</Tip>

**`max_entries`参数**

`max_entries`参数设置缓存中的最大条目数。限制缓存条目数的上限对于限制内存使用（问题1）特别有用，尤其是在缓存大对象时。当缓存已满时，最旧的条目将在添加新条目时被移除。以下是一个示例：

```python
@st.cache_data(max_entries=1000)  # 👈 Maximum 1000 entries in the cache
def get_large_array(seed):
    np.random.seed(seed)
    arr = np.random.rand(100000)
    return arr
```

### 自定义加载图标

默认情况下，当缓存函数正在运行时，Streamlit会在应用程序中显示一个小的加载图标。您可以使用`show_spinner`参数轻松修改它，该参数适用于缓存装饰器的两种方式：

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

在缓存函数中，所有的输入参数必须是可哈希的。让我们快速解释一下为什么以及它的含义。当函数被调用时，Streamlit会查看它的参数值以确定它之前是否已被缓存过。因此，它需要一种可靠的方法来比较函数调用之间的参数值。对于字符串或整数来说很简单，但对于任意对象来说就复杂了！Streamlit使用[哈希函数](https://en.wikipedia.org/wiki/Hash_function)来解决这个问题。它将参数转换为一个稳定的键并存储该键。在下一次函数调用时，它会再次对参数进行哈希并将其与存储的哈希键进行比较。

很不幸，不是所有的参数都是可哈希的！例如，您可能会传递一个不可哈希的数据库连接或机器学习模型给您的缓存函数。在这种情况下，您可以排除输入参数进行缓存。只需在参数名称前加下划线（例如 `_param1`），它就不会用于缓存。即使它发生了变化，如果所有其他参数匹配，Streamlit也会返回缓存的结果。

这里是一个示例：

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

如我们所知，Streamlit只在之前没有缓存的情况下运行这个函数。在第一次运行时，`st.success`消息会在应用程序中显示。但是在后续运行中会发生什么呢？它仍然会显示出来！Streamlit意识到在缓存的函数中存在`st.`命令，它在第一次运行时保存它，并在后续运行时重新播放。对于缓存装饰器，重新播放静态元素也是有效的。

您还可以使用此功能来缓存整个用户界面的部分内容：

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

您还可以在缓存函数中使用交互式输入小部件，如`st.slider`或`st.text_input`。小部件回放目前是一个实验性功能。要启用它，您需要设置`experimental_allow_widgets`参数：

```python
@st.cache_data(experimental_allow_widgets=True)  # 👈 Set the parameter
def get_data():
    num_rows = st.slider("Number of rows to get")  # 👈 Add a slider
    data = api.get(..., num_rows)
    return data
```

Streamlit将滑块视为缓存函数的附加输入参数。如果您更改滑块的位置，Streamlit会检查是否已经为此滑块值缓存了函数。如果是，则会返回缓存的值。如果没有，则会使用新的滑块值重新运行函数。

在缓存函数中使用小部件非常强大，因为它可以缓存应用程序的整个部分。但是这也可能存在一定的风险！由于Streamlit将小部件值视为额外的输入参数，它很容易导致内存使用过多。想象一下，如果您的缓存函数有五个滑块并返回一个100MB的数据帧，那么对于这五个滑块值的每个排列，我们都会将100MB添加到缓存中——即使滑块不影响返回的数据！这些添加操作会使您的缓存快速膨胀。如果您在缓存函数中使用小部件，请注意这个限制。我们建议您仅在UI的隔离部分中使用此功能，其中小部件直接影响缓存的返回值。

<警告>

对于缓存函数中的小部件支持是实验性的。我们可能随时更改或删除它，而不会提前警告。请谨慎使用！
</警告>

<注意>

目前在缓存函数中不支持两个小部件：`st.file_uploader`和`st.camera_input`。我们可能会在将来支持它们。如果您需要这些功能，请随时[提交GitHub问题](https://github.com/streamlit/streamlit/issues)！
</注意>

### 处理大数据

正如我们所解释的，您应该使用`st.cache_data`来缓存数据对象。但是对于非常大的数据，例如具有超过1亿行的DataFrames或数组，这可能会很慢。这是因为`st.cache_data`的[复制行为](#copying-behavior)：在第一次运行时，它将返回值序列化为字节，并在后续运行中进行反序列化。这两个操作都需要时间。

如果您处理的是非常大的数据，使用`st.cache_resource`可能是个不错的选择。它不会通过序列化/反序列化创建返回值的副本，几乎是即时的。但要注意：对函数返回值的任何更改（例如从DataFrame中删除列或在数组中设置值）直接操作缓存中的对象。您必须确保这不会损坏您的数据或导致崩溃。请参见下面的[变异和并发问题](#mutation-and-concurrency-issues)部分。

在对具有四列的pandas DataFrame进行`st.cache_data`基准测试时，我们发现当行数超过1亿行时，速度变慢。下表显示了不同行数（都具有四列）下两种缓存装饰器的运行时：

|                   |                 | 1000万行 | 5000万行 | 1亿行 | 2亿行 |
| ----------------- | --------------- | :------: | :------: | :-------: | :-------: |
| st.cache_data     | 第一次运行\*     |  0.4秒   |   3秒    |   14秒    |   28秒    |
|                   | 后续运行 |  0.2秒   |   1秒    |    2秒    |    7秒    |
| st.cache_resource | 第一次运行\*     |  0.01秒  |  0.1秒   |   0.2秒   |    1秒    |
|                   | 后续运行 |   0秒    |   0秒    |    0秒    |    0秒    |

|                                                                                                                                                              |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _\*在第一次运行时，表格只显示使用缓存装饰器的开销时间，不包括缓存函数本身的运行时间。_ |

### 突变和并发问题

在上面的部分中，我们详细讨论了在缓存函数的返回对象上进行突变时可能出现的问题。这个话题很复杂！但它对于理解`st.cache_data`和`st.cache_resource`之间的行为差异至关重要。让我们深入一些。

首先，我们应该明确定义我们所说的突变和并发：

- 通过**突变（mutations）**，我们指的是在调用函数之后对缓存函数的返回值进行的任何更改。例如：

  ```python
  @st.cache_data
  def create_list():
      l = [1, 2, 3]

  l = create_list()  # 👈 调用函数
  l[0] = 2  # 👈 更改其返回值
  ```

- **并发性**，我们指的是多个会话可以同时引起这些变异。Streamlit是一个需要处理许多用户和会话连接到应用程序的网络框架。如果两个人同时查看一个应用程序，它们都会导致Python脚本重新运行，这可能会同时操作缓存的返回对象 - 并发地。

修改缓存返回对象可能是危险的。它可能导致应用程序出现异常，甚至损坏数据（这可能比应用程序崩溃更糟糕！）。在下面，我们首先解释`st.cache_data`的复制行为，并展示如何避免突变问题。然后，我们将展示并发突变如何导致数据损坏以及如何防止它。

#### 复制行为

`st.cache_data` 在每次调用函数时都会创建缓存返回值的副本。这样可以避免大部分变异和并发问题。为了详细了解它，请回顾一下上面关于 `st.cache_data` 的[Uber共享乘车示例](#usage)。我们对它进行了两个修改：

1. 我们使用 `st.cache_resource` 而不是 `st.cache_data`。`st.cache_resource` **不会**创建缓存对象的副本，这样我们可以看到没有复制行为时会发生什么情况。
2. 加载数据后，我们通过删除`"Lat"`列来对返回的DataFrame进行操作（就地操作）。

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

让我们运行它，看看会发生什么！第一次运行应该可以正常工作。但在第二次运行时，你会看到一个异常：`KeyError: "['Lat'] not found in axis"`。为什么会发生这种情况？让我们一步一步来解释：

- 在第一次运行时，Streamlit运行`load_data`并将结果的DataFrame存储在缓存中。由于我们使用了`st.cache_resource`，它**不会**创建副本，而是存储原始的DataFrame。
- 然后我们从DataFrame中删除“Lat”列。请注意，这是从缓存中存储的_原始_ DataFrame 中删除列。我们正在对其进行操作！
- 在第二次运行时，Streamlit从缓存中返回完全相同的操作后的DataFrame。它不再具有“Lat”列！因此，我们对`df.drop`的调用会导致异常。Pandas无法删除不存在的列。

`st.cache_data`的复制行为可以防止这种变异错误。变异只会影响一个特定的副本，而不是缓存中的底层对象。下一次重新运行会得到自己的、未经变异的DataFrame副本。您可以自己尝试一下，只需将上面的`st.cache_resource`替换为`st.cache_data`，您会发现一切都正常工作。

由于这种复制行为，`st.cache_data`是推荐的缓存数据转换和计算的方式 - 任何返回可串行化对象的操作。

#### 并发问题

现在让我们看一下当多个用户同时对缓存中的对象进行变更时可能发生的情况。假设您有一个返回列表的函数。同样，我们使用 `st.cache_resource` 来对其进行缓存，以便我们不会创建副本：

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

假设用户A运行该应用程序。他们将看到以下输出:

```python
l[0] is: 2
```

假设另一个用户B在之后访问该应用程序。与用户A相比，他们将看到以下输出:

```python
l[0] is: 3
```

现在，用户A在用户B之后立即重新运行应用程序。他们将看到以下输出：

```python
l[0] is: 4
```

发生了什么？为什么所有的输出都不同？

- 当用户A访问应用程序时，调用`create_list()`，并将列表`[1, 2, 3]`存储在缓存中。然后将此列表返回给用户A。列表的第一个值`1`被赋给`first_list_value`，并且`l[0]`被更改为`2`。
- 当用户B访问应用程序时，`create_list()`从缓存中返回了被修改的列表：`[2, 2, 3]`。列表的第一个值`2`被赋给`first_list_value`，并且`l[0]`被更改为`3`。
- 当用户A重新运行应用程序时，`create_list()`再次返回变异的列表：`[3, 2, 3]`。列表的第一个值`3`被赋给`first_list_value`，并且`l[0]`被更改为4。

如果你仔细思考一下，这是有道理的。用户A和用户B使用的是同一个列表对象（存储在缓存中的对象）。由于列表对象被改变，用户A对列表对象的更改也会在用户B的应用程序中反映出来。

这就是为什么在多个用户同时访问应用程序时，必须小心处理使用`st.cache_resource`缓存的可变对象。如果我们使用`st.cache_data`而不是`st.cache_resource`，应用程序将为每个用户复制列表对象，并且上述示例将按预期工作 - 用户A和B都将看到：

```python
l[0] is: 2
```

<注意>

这个玩具示例可能看起来无伤大雅。但是数据损坏可能非常危险！想象一下，如果我们在这里使用了一个大型银行的财务记录，你肯定不希望因为有人使用了错误的缓存装饰器而在账户上醒来时发现少了钱 😉

</注意>

## 从st.cache迁移

我们在Streamlit 1.18.0中引入了上述的缓存命令。在此之前，我们只有一个全能命令`st.cache`。使用它经常会令人困惑，导致奇怪的异常，并且速度较慢。这就是为什么我们在1.18.0中用新的命令来替代`st.cache`（在这篇[博文](https://blog.streamlit.io/introducing-two-new-caching-commands-to-replace-st-cache/)中了解更多）。新命令提供了更直观和高效的方式来缓存您的数据和资源，并且旨在在所有新的开发中取代`st.cache`。

如果您的应用程序仍在使用`st.cache`，请不要担心！以下是一些关于迁移的注意事项：

- `st.cache`已被弃用。• Streamlit的新版本将在应用程序使用它时显示废弃警告。
- 我们不会立即删除`st.cache`，所以您不必担心您两年前的应用程序会出现问题。但是我们鼓励您使用新的命令，它们将会更加方便！
- 在大多数情况下，切换代码到新的命令应该很容易。要决定是使用 `st.cache_data` 还是 `st.cache_resource`，请阅读[决定使用哪个缓存装饰器](#deciding-which-caching-decorator-to-use)。Streamlit还会识别常见用例，并在废弃警告中显示提示信息。
- 大多数 `st.cache` 的参数也存在于新的命令中，但有几个例外情况：
  - `allow_output_mutation` 不再存在。您可以安全地将其删除。只需确保为您的用例使用正确的缓存命令。
  - `suppress_st_warning` 不再存在。您可以安全地将其删除。缓存函数现在可以包含 Streamlit 命令并重放它们。如果您想在缓存函数内使用小部件，请设置 `experimental_allow_widgets=True`。请参阅 [这里](#using-streamlit-commands-in-cached-functions)。
  - `hash_funcs`不再存在。您可以通过在参数前加下划线来排除参数的缓存（和哈希）：`_excluded_param`。请参阅[这里](#excluding-input-parameters)。

如果在迁移过程中有任何问题或疑问，请在[论坛](https://discuss.streamlit.io/)上与我们联系，我们将乐意帮助您。🎈