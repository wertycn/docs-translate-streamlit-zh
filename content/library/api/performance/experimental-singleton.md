---
description: st.experimental_singleton is a function decorator used to store singleton
  objects.
slug: /library/api-reference/performance/st.experimental_singleton
title: st.experimental_singleton
---

<重要提示>

这是一个实验性功能。实验性功能及其API可能随时更改或删除。了解更多信息，请点击[这里](/library/advanced-features/prerelease#experimental-features)。

</重要提示>

<Autofunction function="streamlit.experimental_singleton" deprecated={true} deprecatedText="<code>st.experimental_singleton</code> 在版本1.18.0中已被弃用。请改用<a href='/library/api-reference/performance/st.cache_resource'><code>st.cache_resource</code></a>。在<a href='/library/advanced-features/caching'>缓存</a>中了解更多信息。"/>

### 验证缓存

`@st.experimental_singleton` 装饰器用于缓存函数的输出，这样只需要执行一次。在某些情况下，这可以提高性能，比如当函数执行时间较长或者需要进行网络请求时。

然而，在某些情况下，缓存的输出可能会随着时间的推移变得无效，例如当数据库连接超时时。为了处理这种情况，`@st.experimental_singleton` 装饰器支持一个可选的 `validate` 参数，它接受一个验证函数，在每次访问缓存的输出时被调用。如果验证函数返回 False，则缓存的输出被丢弃，并且装饰的函数再次执行。

<!-- 让我们学习如何使用 `validate` 参数来确保缓存输出的有效性。同时，让我们看一些具体的例子和最佳实践，帮助您更好地理解如何有效地使用这个功能。

#### 示例 1：验证数据库连接

在下面的示例中，我们使用`psycopg2`库连接到一个[公共的PostgreSQL数据库](https://rnacentral.org/help/public-database)。我们使用`@st.experimental_singleton`来缓存连接，并使用`validate`参数在返回连接之前进行验证。如果连接无效，我们重新连接并返回新的连接。为了模拟连接超时，我们使用一个复选框来关闭连接。当连接关闭时，缓存的值被丢弃并重新建立连接。

```python
import psycopg2
import streamlit as st

if "logs" not in st.session_state:
    st.session_state.logs = []

# Function to validate psycopg2 connection
def validate_connection(conn):
    try:
        # Check if connection is valid
        # by executing a simple query
        with conn.cursor() as curs:
            curs.execute("SELECT 1")
            curs.fetchone()
    except:
        st.session_state.logs.append("Connection lost. Reconnecting...")
        # Connection is invalid, invalidate cache
        return False
    return True # Connection is valid, don't invalidate cache

# Get connection parameters
host = "hh-pgsql-public.ebi.ac.uk"
dbname = "pfmegrnargs"
port = 5432
user = "reader"
password = "NWDMCE5xdipIjRrp"

# Initialize connection
# Connection will be reinitialized if validation function returns False.
@st.experimental_singleton(validate=validate_connection)
def init_connection():
    # Connect to the database
    conn = psycopg2.connect(
        host=host,
        dbname=dbname,
        port=port,
        user=user,
        password=password,
    )
    return conn

conn = init_connection()

# Create a cursor object
cursor = conn.cursor()

# Execute query
cursor.execute("SELECT * FROM rnc_database ORDER BY id LIMIT 10")

# Fetch data
data = cursor.fetchall()

# Append to logs
st.session_state.logs.append("Data fetched successfully.")

# Create a Dataframe
st.dataframe(data)

# Display logs
st.write(st.session_state.logs)

# Checkbox to close connection
if st.checkbox("Close connection"):
    cursor.close()
    conn.close()
    st.session_state.logs.append("Connection closed.")

st.button("Rerun")
```

首先，在不关闭连接的情况下尝试运行应用程序。您应该看到一个包含10行数据的表格，并显示"数据获取成功"的消息。

现在，勾选复选框以关闭连接，然后点击"重新运行"。您应该会看到一条消息，显示"连接丢失，正在重新连接..."，以及一个新的包含10行数据的表格。这是因为缓存的连接被丢弃，重新建立了一个新的连接。

接下来，从装饰器中删除 `validate=validate_connection` 参数，并重新运行应用程序。您应该会看到一个错误消息，提示 `InterfaceError: connection already closed`。这是因为缓存的连接未经验证，即使已关闭也会返回。

在这里，我们通过关闭连接来模拟连接超时。在实际应用中，可以通过移除超时模拟并使用`validate`参数来验证连接，如下所示：

```python
import psycopg2
import streamlit as st

# Function to validate psycopg2 connection
def check_connection(conn):
    try:
        # Check if connection is valid
        conn.poll()
        return True # Connection is valid, don't invalidate cache
    except psycopg.OperationalError:
        return False # Connection is invalid, invalidate cache

# Get connection parameters
host = "hh-pgsql-public.ebi.ac.uk"
dbname = "pfmegrnargs"
port = 5432
user = "reader"
password = "NWDMCE5xdipIjRrp"

# Initialize and cache connection.
# Connection will be reinitialized if validation function returns False.
@st.experimental_singleton(validate=check_connection)
def init_connection():
    # Connect to the database
    conn = psycopg2.connect(
        host=host,
        dbname=dbname,
        port=port,
        user=user,
        password=password,
    )
    return conn

conn = init_connection()

# Create a cursor object
cursor = conn.cursor()

# Execute query
cursor.execute("SELECT * FROM rnc_database ORDER BY id LIMIT %s", (10,))

# Fetch data
data = cursor.fetchall()

# Create a Dataframe
st.dataframe(data)
```

在这个例子中，我们使用`validate`参数来验证数据库连接。然而，您可以使用该参数来验证任何类型的缓存输出，例如网络请求或文件。

#### 示例2：验证API响应

假设您有一个函数`get_api_data()`，它从一个API获取一些数据。如果响应无效，该API将返回500错误。我们使用`validate`参数来检查响应对象的`status_code`属性，当API返回成功响应时，该属性被设置为200。如果状态码不是200，验证函数将返回`False`，然后再次执行`get_api_data()`函数，重新从API获取数据。

下面是一个使用validate参数验证API响应的工作示例：

```python
import requests

import streamlit as st

if "status_code" not in st.session_state:
    st.session_state.status_code = "200"

if "logs" not in st.session_state:
    st.session_state.logs = []

# A function to validate the response
def validate_response(response):
    if response.status_code == int(st.session_state.status_code):
        return True
    else:
        st.session_state.logs.append("Invalid response. Reconnecting...")
        return False

@st.experimental_singleton(validate=validate_response)
def get_api_data():
    url = "https://jsonplaceholder.typicode.com/posts/1"
    response = requests.get(url)
    return response

data = get_api_data().json()
st.session_state.logs.append("Data fetched successfully.")
st.write(data)

# simulate an invalid API response
if st.checkbox("Simulate invalid API response"):
    st.session_state.status_code = "500"
else:
    st.session_state.status_code = "200"

st.button("Rerun")
st.subheader("Logs")
st.write(st.session_state.logs)
```

示例使用一个公共的API端点，返回一个包含帖子信息的JSON对象，并通过将验证函数更改为在`status_code`不是`200`时返回`False`来模拟一个无效的API响应，这是在复选框被选中后进行的。

在这个示例中，首先运行应用程序而不模拟无效的API响应。您应该看到从API返回的数据以及显示"数据获取成功"的消息。

现在，勾选复选框以模拟无效的API响应，然后点击"重新运行"。您应该会看到一条消息，上面写着"无效的响应。正在重新连接..."，以及来自API的新数据。这是因为缓存的响应被丢弃了，`get_api_data()`函数被再次执行。

接下来，从`@st.experimental_singleton`装饰器中移除`validate=validate_response`参数，并重新运行应用程序。您应该会看到一个错误消息，提示"NoneType对象的 'status_code' 属性"。这是因为缓存的响应未经验证，即使是无效的也会返回。

通过在`@st.experimental_singleton`装饰器中使用`validate`参数，您可以确保缓存的数据保持有效，并防止由于过时或无效数据而引起的错误。

如果我们在上面的示例中不使用模拟，并使用`validate`参数来验证API响应，代码可以简化为：

```python
import requests
import streamlit as st

@st.experimental_singleton(
    validate=lambda response: True if response.status_code == 200 else False
)
def get_api_data():
    url = "https://jsonplaceholder.typicode.com/posts/1"
    response = requests.get(url)
    return response

try:
    response = get_api_data()
    response.raise_for_status()
    data = response.json()
    st.write(data)
except requests.exceptions.HTTPError as err:
    st.error(err)
``` -->

#### 最佳实践

- 当缓存的输出可能随时间无效时，例如数据库连接或API密钥过期时，请使用`validate`参数。
- 谨慎使用`validate`参数，因为它会增加每次访问缓存输出时调用验证函数的额外开销。
- 确保验证函数尽可能快速，因为每次访问缓存输出时都会调用它。
- 考虑定期验证缓存数据，而不是每次访问时验证，以减轻性能影响。
- 处理验证过程中可能发生的错误，并提供备选机制以应对验证失败的情况。

### 在缓存装饰的函数中重新播放静态 `st` 元素

使用`@st.experimental_singleton`装饰的函数可以包含静态的`st`元素。当执行经过缓存装饰的函数时，我们记录产生的元素和阻塞消息，因此即使函数的执行被跳过，结果已经被缓存，这些元素也会出现在应用程序中。

在下面的示例中，使用`@st.experimental_singleton`装饰器来缓存`get_model`函数的执行结果，该函数返回一个🤗 Hugging Face Transformers模型。请注意，缓存的函数中还包含一个`st.bar_chart`命令，当函数被跳过执行时，该命令将被重新执行，因为结果已被缓存。

```python
import numpy as np
import pandas as pd
import streamlit as st
from transformers import AutoModel

@st.experimental_singleton
def get_model(model_type):
    # Contains a static element st.bar_chart
    st.bar_chart(
        np.random.rand(10, 1)
    )  # This will be recorded and displayed even when the function is skipped

    # Create a model of the specified type
    return AutoModel.from_pretrained(model_type)

bert_model = get_model("distilbert-base-uncased")
st.help(bert_model) # Display the model's docstring
```

`cache-decorated` 函数中支持的静态`st`元素包括：

- `st.alert`
- `st.altair_chart`
- `st.area_chart`
- `st.audio`
- `st.bar_chart`
- `st.ballons`
- `st.bokeh_chart`
- `st.caption`
- `st.code`
- `st.components.v1.html`
- `st.components.v1.iframe`
- `st.container`
- `st.dataframe`
- `st.echo`
- `st.empty`
- `st.error`
- `st.exception`
- `st.expander`
- `st.experimental_get_query_params`
- `st.experimental_set_query_params`：设置查询参数
- `st.form`：创建表单
- `st.form_submit_button`：创建表单提交按钮
- `st.graphviz_chart`：绘制Graphviz图表
- `st.help`：显示帮助信息
- `st.image`：显示图片
- `st.info`：显示信息
- `st.json`：显示JSON数据
- `st.latex`：显示LaTeX公式
- `st.line_chart`：绘制折线图
- `st.markdown`：显示Markdown文本
- `st.metric`：显示度量值
- `st.plotly_chart`：绘制Plotly图表
- `st.progress`：显示进度条
- `st.pydeck_chart`：绘制PyDeck图表
- `st.snow`：显示下雪效果
- `st.spinner`：显示加载动画
- `st.success`：显示成功消息
- `st.table`：显示表格
- `st.text`：显示文本
- `st.vega_lite_chart`：绘制Vega-Lite图表
- `st.video`：显示视频
- `st.warning`：显示警告信息

### 在缓存修饰函数中重放输入部件

除了静态元素之外，使用`@st.experimental_singleton`装饰的函数还可以包含[input widgets](/library/api-reference/widgets)！回放输入小部件默认是禁用的。要启用它，您可以为`@st.experimental_singleton`设置`experimental_allow_widgets`参数为`True`。下面的示例启用了小部件回放，并展示了在使用缓存装饰的函数中使用复选框小部件的示例。

```python
import streamlit as st

# Enable widget replay
@st.experimental_singleton(experimental_allow_widgets=True)
def func():
    # Contains an input widget
    st.checkbox("Works!")

func()
```

如果被缓存装饰的函数包含输入小部件，但`experimental_allow_widgets`设置为`False`或未设置，Streamlit会抛出`CachedStFunctionWarning`警告，如下所示：

```python
import streamlit as st

# Widget replay is disabled by default
@st.experimental_singleton
def func():
    # Streamlit will throw a CachedStFunctionWarning
    st.checkbox("Doesn't work")

func()
```

![缓存状态函数警告小部件](/images/cached-st-function-warning-widgets.png)

### 小部件回放原理

让我们来揭开缓存修饰函数中小部件回放的神秘面纱，并获得概念上的理解。小部件值被视为函数的额外输入，并用于确定函数是否应该执行。考虑以下示例：

```python
import streamlit as st

@st.experimental_singleton(experimental_allow_widgets=True)
def plus_one(x):
    y = x + 1
    if st.checkbox("Nuke the value 💥"):
        st.write("Value was nuked, returning 0")
        y = 0
    return y

st.write(plus_one(2))
```

`plus_one` 函数接受一个整数 `x` 作为输入，并返回 `x + 1`。该函数还包含一个复选框小部件，用于"清零" `x` 的值。即 `plus_one` 的返回值取决于复选框的状态：如果选中，函数返回 `0`，否则返回 `3`。

为了确定缓存应该返回哪个值（在缓存命中的情况下），Streamlit将复选框的状态（选中/未选中）视为函数`plus_one`的一个附加输入（就像`x`一样）。如果用户选中复选框（从而改变其状态），我们会查找具有相同`x`值（2）和相同复选框状态（选中）的缓存。如果缓存中包含这个输入组合的值，则返回该值。否则，我们执行函数并将结果存储在缓存中。

现在让我们了解一下启用和禁用小部件回放如何改变函数的行为。

#### 禁用小部件回放

- 缓存函数中的小部件会抛出`CachedStFunctionWarning`警告，并被忽略。
- 缓存函数中的其他静态元素会按预期进行回放。

#### 启用小部件回放

- 缓存函数中的小部件不会导致警告，并会按预期进行回放。
- 在缓存函数中与小部件交互将导致函数再次执行，并更新缓存。
- 缓存函数中的小部件在重新运行时保留其状态。
- 每个不同的小部件值组合被视为函数的单独输入，并用于确定是否应该执行函数。即，每个不同的小部件值组合都有自己的缓存条目；缓存的函数在第一次运行时执行，并在之后使用保存的值。
- 在一个脚本运行中多次使用相同参数调用缓存的函数会触发`DuplicateWidgetID`错误。
- 如果缓存函数的参数发生变化，再次渲染的小部件将保留它们的状态。
- 更改缓存函数的源代码将使缓存失效。
- [`st.experimental_singleton`](/library/api-reference/performance/st.experimental_singleton) 和 [`st.experimental_memo`](/library/api-reference/performance/st.experimental_memo) 都支持小部件重放。
- 从根本上讲，使用（支持的）小部件的函数在被 `@st.experimental_singleton` 或 `@st.experimental_memo` 装饰后的行为不会发生变化。唯一的区别是只有在检测到缓存 "miss" 时才执行该函数。

### 支持的小部件

所有的输入小部件都支持在缓存装饰的函数中使用。以下是支持的小部件的详细列表：

- `st.button`
- `st.camera_input`
- `st.checkbox`
- `st.color_picker`
- `st.date_input`
- `st.download_button`
- `st.file_uploader`
- `st.multiselect`
- `st.number_input`
- `st.radio`
- `st.selectbox`
- `st.select_slider`
- `st.slider`
- `st.text_area`
- `st.text_input`
- `st.time_input`