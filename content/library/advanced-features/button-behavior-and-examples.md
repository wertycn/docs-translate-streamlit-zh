---
title: 按钮行为和示例
slug: /library/advanced-features/button-behavior-and-examples
---

# 按钮行为和示例

## 概述

使用[`st.button`](/library/api-reference/widgets/st.button)创建的按钮不会保留状态。在按钮被点击后，脚本重新运行时，按钮会返回`True`，然后在下一次脚本重新运行时立即返回`False`。如果一个显示的元素嵌套在`if st.button('Click me'):`内部，那么在按钮被点击时，该元素将可见，并且在用户进行下一个操作时会消失。这是因为脚本重新运行，按钮变为`False`。

在本指南中，我们将介绍如何使用按钮，并解释常见的误解。继续阅读，了解使用`st.button`和[`st.session_state`](/library/api-reference/session-state)的各种示例。请打开您喜欢的代码编辑器，以便在阅读的同时使用`streamlit run`运行示例。如果您还没有运行过自己的Streamlit脚本，请查看Streamlit的[主要概念](/library/get-started/main-concepts)。

## 何时使用`if st.button()`

当代码以按钮的值为条件时，它会在按钮被点击后执行一次，并且在下一次点击按钮之前不会再次执行。

适合嵌套在按钮内部的内容：

- 瞬态消息，即立即消失的消息。
- 每次点击时保存数据到会话状态、文件或数据库的处理过程。

不适合嵌套在按钮内部的内容：

- 用户继续操作时应保持持久性的显示项。
- 导致脚本在使用时重新运行的其他小部件。
- 不修改会话状态或写入文件/数据库的过程。\*

\* 当需要一次性结果时，这可能是合适的。如果您有一个“验证”按钮，那么可以直接对该按钮进行条件处理。它可以用来创建一个警告，显示“有效”或“无效”的信息，而无需保留该信息。

## 与按钮相关的常见逻辑

### 使用按钮显示临时消息

如果您想为用户提供一个快速按钮来检查条目是否有效，但不会在用户继续操作时保持该检查的显示。

在这个例子中，用户可以点击一个按钮来检查他们的`animal`字符串是否在`animal_shelter`列表中。当用户点击“**检查可用性**”时，他们将看到“我们有这个动物！”或“我们没有这个动物。”如果他们在[`st.text_input`](/library/api-reference/widgets/st.text_input)中更改了动物，脚本会重新运行并且消息会消失，直到他们再次点击“**检查可用性**”。

```python
import streamlit as st

animal_shelter = ['cat', 'dog', 'rabbit', 'bird']

animal = st.text_input('Type an animal')

if st.button('Check availability'):
    have_it = animal.lower() in animal_shelter
    'We have that animal!' if have_it else 'We don\'t have that animal.'
```

注意：上面的示例使用[magic](/library/api-reference/write-magic/magic)在前端渲染消息。

### 有状态的按钮

如果你希望一个被点击的按钮继续保持为`True`，可以在回调中创建一个`st.session_state`的值，并使用按钮将其设置为`True`。

```python
import streamlit as st

if 'clicked' not in st.session_state:
    st.session_state.clicked = False

def click_button():
    st.session_state.clicked = True

st.button('Click me', on_click=click_button)

if st.session_state.clicked:
    # The message and nested widget will remain on the page
    st.write('Button clicked!')
    st.slider('Select a value')
```

### 切换按钮

如果您希望按钮像切换开关一样工作，将回调函数设置为反转保存在`st.session_state`中的布尔值。

在此示例中，我们使用`st.button`来切换另一个小部件的开关状态。通过在`st.session_state`中的一个值上有条件地显示[`st.slider`](/library/api-reference/widgets/st.slider)，用户可以与滑块进行交互而不会使其消失。

```python
import streamlit as st

if 'button' not in st.session_state:
    st.session_state.button = False

def click_button():
    st.session_state.button = not st.session_state.button

st.button('Click me', on_click=click_button)

if st.session_state.button:
    # The message and nested widget will remain on the page
    st.write('Button is on!')
    st.slider('Select a value')
else:
    st.write('Button is off!')
```

或者，您可以在滑块的`disabled`参数中使用`st.session_state`中的值。

```python
import streamlit as st

if 'button' not in st.session_state:
    st.session_state.button = False

def click_button():
    st.session_state.button = not st.session_state.button

st.button('Click me', on_click=click_button)

st.slider('Select a value', disabled=st.session_state.button)
```

### 用于继续或控制过程阶段的按钮

将内容嵌套在按钮内部的另一种选择是使用`st.session_state`中的值来指定过程的“步骤”或“阶段”。 在这个例子中，我们的脚本有四个阶段：0. 用户开始之前。

1. 用户输入他们的名字。
2. 用户选择一种颜色。
3. 用户收到一条感谢信息。
   开始时的按钮将舞台从0推进到1。结束时的按钮将舞台从3重置为0。舞台1和2中使用的其他小部件具有回调函数来设置舞台。如果您有一个有依赖步骤的流程，并且希望保持先前的舞台可见，这样的回调将迫使用户重新回溯后续的舞台，如果他们更改了先前的小部件。

```python
import streamlit as st

if 'stage' not in st.session_state:
    st.session_state.stage = 0

def set_state(i):
    st.session_state.stage = i

if st.session_state.stage == 0:
    st.button('Begin', on_click=set_state, args=[1])

if st.session_state.stage >= 1:
    name = st.text_input('Name', on_change=set_state, args=[2])

if st.session_state.stage >= 2:
    st.write(f'Hello {name}!')
    color = st.selectbox(
        'Pick a Color',
        [None, 'red', 'orange', 'green', 'blue', 'violet'],
        on_change=set_state, args=[3]
    )
    if color is None:
        set_state(2)

if st.session_state.stage >= 3:
    st.write(f':{color}[Thank you!]')
    st.button('Start Over', on_click=set_state, args=[0])
```

### 修改 `st.session_state` 的按钮

如果您在按钮内部修改 `st.session_state`，您必须考虑该按钮在脚本中的位置。

#### 一个小问题

在这个示例中，我们在修改它之前和之后都访问 `st.session_state.name` 。当点击按钮（"**Jane**" 或 "**John**"）时，脚本会重新运行。按钮之前显示的信息滞后于按钮之后写入的信息。按钮之前的 `st.session_state` 中的数据没有被更新。当脚本执行按钮函数时，即更新 `st.session_state` 的条件代码创建了变化。因此，这个变化会在按钮之后反映出来。

```python
import streamlit as st
import pandas as pd

if 'name' not in st.session_state:
    st.session_state['name'] = 'John Doe'

st.header(st.session_state['name'])

if st.button('Jane'):
    st.session_state['name'] = 'Jane Doe'

if st.button('John'):
    st.session_state['name'] = 'John Doe'

st.header(st.session_state['name'])
```

#### 在带有重新运行功能的按钮中嵌套逻辑

如果需要在修改数据之前访问`st.session_state`中的数据，可以在修改已提交后再次运行脚本时，包含`st.experimental_rerun`。

```python
import streamlit as st
import pandas as pd

if 'name' not in st.session_state:
    st.session_state['name'] = 'John Doe'

st.header(st.session_state['name'])

if st.button('Jane'):
    st.session_state['name'] = 'Jane Doe'
    st.experimental_rerun()

if st.button('John'):
    st.session_state['name'] = 'John Doe'
    st.experimental_rerun()

st.header(st.session_state['name'])
```

#### 回调函数中使用的逻辑

回调函数是一种修改 `st.session_state` 的简洁方式。回调函数在脚本重新运行之前被执行，因此按钮相对于访问数据的位置并不重要。

```python
import streamlit as st
import pandas as pd

if 'name' not in st.session_state:
    st.session_state['name'] = 'John Doe'

def change_name(name):
    st.session_state['name'] = name

st.header(st.session_state['name'])

st.button('Jane', on_click=change_name, args=['Jane Doe'])
st.button('John', on_click=change_name, args=['John Doe'])

st.header(st.session_state['name'])
```

### 修改或重置其他小部件的按钮

当使用按钮来修改或重置其他小部件时，与上面的示例一样，可以通过修改`st.session_state`来实现。然而，还存在一个额外的考虑因素：如果具有该键的小部件已经在当前脚本运行的页面上呈现过了，就不能修改`st.session_state`中的键值对。

<重要提示>

不要这样做！

```python
import streamlit as st

st.text_input('Name', key='name')

# These buttons will error because they change a key after its widget
if st.button('Clear name'):
    st.session_state.name = ''
if st.button('Streamlit!'):
    set_name('Streamlit')
```

</Important>

#### 选项1：为按钮使用一个键，并将逻辑放在小部件之前

如果您为按钮分配一个键，您可以通过在`st.session_state`中使用其值来条件编码按钮的状态。这意味着依赖于您的按钮的逻辑可以在该按钮之前的脚本中。在下面的示例中，我们使用`st.session_state`上的`.get()`方法，因为当脚本第一次运行时，按钮的键将不存在。如果无法找到键，则`.get()`方法将返回`False`。否则，它将返回键的值。

```python
import streamlit as st

# Use the get method since the keys won't be in session_state
# on the first script run
if st.session_state.get('clear'):
    st.session_state['name'] = ''
if st.session_state.get('streamlit'):
    st.session_state['name'] = 'Streamlit'

st.text_input('Name', key='name')

st.button('Clear name', key='clear')
st.button('Streamlit!', key='streamlit')
```

#### 选项2：使用回调函数

```python
import streamlit as st

st.text_input('Name', key='name')

def set_name(name):
    st.session_state.name = name

st.button('Clear name', on_click=set_name, args=[''])
st.button('Streamlit!', on_click=set_name, args=['Streamlit'])
```

#### 选项3：使用容器

通过使用[`st.container`](/library/api-reference/layout/st.container)，您可以使小部件在脚本和前端视图（网页）中以不同的顺序出现。

```python
import streamlit as st

begin = st.container()

if st.button('Clear name'):
    st.session_state.name = ''
if st.button('Streamlit!'):
    st.session_state.name = ('Streamlit')

# The widget is second in logic, but first in display
begin.text_input('Name', key='name')
```

### 动态添加其他小部件的按钮

当动态添加小部件到页面时，请确保使用索引来保持键的唯一性，避免出现“DuplicateWidgetID”错误。在这个例子中，我们定义了一个名为`display_input_row`的函数，它用于渲染一行小部件。该函数接受一个`index`作为参数。由`display_input_row`渲染的小部件在其键中使用`index`，以便在单个脚本重新运行时可以多次执行`display_input_row`而不重复任何小部件键。

```python
import streamlit as st

def display_input_row(index):
    left, middle, right = st.columns(3)
    left.text_input('First', key=f'first_{index}')
    middle.text_input('Middle', key=f'middle_{index}')
    right.text_input('Last', key=f'last_{index}')

if 'rows' not in st.session_state:
    st.session_state['rows'] = 0

def increase_rows():
    st.session_state['rows'] += 1

st.button('Add person', on_click=increase_rows)

for i in range(st.session_state['rows']):
    display_input_row(i)

# Show the results
st.subheader('People')
for i in range(st.session_state['rows']):
    st.write(
        f'Person {i+1}:',
        st.session_state[f'first_{i}'],
        st.session_state[f'middle_{i}'],
        st.session_state[f'last_{i}']
    )
```

### 处理昂贵或文件写入过程的按钮

当您有耗时的进程时，可以将它们设置为在点击按钮时运行，并将结果保存到`st.session_state`中。这样，您可以在不必要重新执行进程的情况下继续访问进程的结果。这对于保存到磁盘或写入数据库的进程特别有帮助。在这个示例中，我们有一个`expensive_process`，它依赖于两个参数：`option`和`add`。在功能上，`add`会更改输出，但`option`不会&mdash;`option`只是提供一个参数。

```python
import streamlit as st
import pandas as pd
import time

def expensive_process(option, add):
    with st.spinner('Processing...'):
        time.sleep(5)
    df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6], 'C':[7, 8, 9]}) + add
    return (df, add)

cols = st.columns(2)
option = cols[0].selectbox('Select a number', options=['1', '2', '3'])
add = cols[1].number_input('Add a number', min_value=0, max_value=10)

if 'processed' not in st.session_state:
    st.session_state.processed = {}

# Process and save results
if st.button('Process'):
    result = expensive_process(option, add)
    st.session_state.processed[option] = result

if option in st.session_state.processed:
    st.write(f'Option {option} processed with add {add}')
    st.write(st.session_state.processed[option][0])
```

敏锐的观察者可能会想到："这有点像缓存。"我们只是保存相对于一个参数的结果，但这种模式可以轻松扩展为保存相对于两个参数的结果。从这个意义上说，是的，它与缓存有一些相似之处，但也有一些重要的区别。当您将结果保存在`st.session_state`中时，结果仅对当前用户在其当前会话中可用。如果您改用[`st.cache_data`](/library/api-reference/performance/st.cache_data)，则结果将对所有用户在所有会话中都可用。此外，如果您想要更新保存的结果，您必须清除该函数的所有保存结果。

## 反模式

以下是一些按钮可能出现问题的简化示例。请注意这些常见错误。

### 按钮嵌套在按钮内部

```python
import streamlit as st

if st.button('Button 1'):
    st.write('Button 1 was clicked')
    if st.button('Button 2'):
        # This will never be executed.
        st.write('Button 2 was clicked')
```

### 按钮内嵌其他小部件

在按钮内部嵌套其他小部件。

```python
import streamlit as st

if st.button('Sign up'):
    name = st.text_input('Name')

    if name:
        # This will never be executed.
        st.success(f'Welcome {name}')
```

### 在按钮中嵌套一个进程而不保存到会话状态中



```python
import streamlit as st
import pandas as pd

file = st.file_uploader("Upload a file", type="csv")

if st.button('Get data'):
    df = pd.read_csv(file)
    # This display will go away with the user's next action.
    st.write(df)

if st.button('Save'):
    # This will always error.
    df.to_csv('data.csv')
```
