---
title: 多页面应用
slug: /library/get-started/multipage-apps
---

# 多页面应用

当应用程序变得庞大时，将其组织成多个页面会变得非常有用。这样可以使开发人员更容易管理应用程序，用户也更容易导航。Streamlit提供了一种无缝创建多页面应用程序的方式。页面会自动显示在应用程序侧边栏的导航小部件中，点击页面将在不重新加载前端的情况下导航到相应页面，使应用程序浏览速度极快！

我们在[上一页](/library/get-started/create-an-app)创建了一个"单页应用"来探索纽约市的Uber公共数据集，了解上下车情况。在本指南中，让我们学习如何创建多页应用。一旦我们对创建多页应用所需的基础有了扎实的掌握，让我们在[下一节](/library/get-started/multipage-apps/create-a-multipage-app)中为自己构建一个多页应用程序！

## 构建多页应用程序的结构

让我们了解创建多页面应用所需的步骤，包括如何定义页面、构建和运行多页面应用程序以及在用户界面中导航页面。一旦您理解了基础知识，您可以直接进入[下一节](/library/get-started/multipage-apps/create-a-multipage-app)，将熟悉的 `streamlit hello` 命令转换成一个多页面应用程序！

## 运行多页面应用程序

运行多页面应用与运行单页面应用相同。运行多页面应用的命令是：

```python
streamlit run [entrypoint file]
```

"入口文件"是应用程序将向用户显示的第一个页面。一旦您在应用程序中添加了页面，入口文件将显示为侧边栏中的顶层页面。您可以将入口文件视为应用程序的"主页"。例如，假设您的入口文件是`Home.py`。那么，要运行您的应用程序，您可以运行`streamlit run Home.py`。这将启动您的应用程序并执行`Home.py`中的代码。

## 添加页面

一旦您创建了入口文件，您可以通过在与入口文件相对的`pages/`目录中创建`.py`文件来添加页面。例如，如果您的入口文件是`Home.py`，那么您可以创建一个`pages/About.py`文件来定义"关于"页面。以下是一个多页面应用程序的有效目录结构示例：

```
Home.py # This is the file you run with "streamlit run"
└─── pages/
  └─── About.py # This is a page
  └─── 2_Page_two.py # This is another page
  └─── 3_😎_three.py # So is this
```

<提示>

在文件名中添加表情符号时，最佳实践是包含一个带有编号前缀，以便在终端中自动完成更加方便。终端自动完成可能会因为表情符号使用的Unicode表示方式而感到困惑。

</提示>

页面被定义为`pages/`目录中的`.py`文件。根据下面的规则，页面的文件名将被转换为侧边栏中的页面名称。例如，`About.py`文件将在侧边栏中显示为"关于"，`2_Page_two.py`将显示为"第二页"，`3_😎_three.py`将显示为“😎 three”:

![目录结构](/images/mpa-add-pages.png)

只有`pages/`目录中的`.py`文件将作为页面加载。Streamlit会忽略`pages/`目录和子目录中的其他所有文件。

## 页面在UI中的标签和排序

侧边栏UI中的页面标签是根据文件名生成的。它们可能与在[`st.set_page_config`](/library/api-reference/utilities/st.set_page_config)中设置的页面标题不同。让我们了解一下什么是有效的页面文件名，页面如何在侧边栏中显示以及页面如何排序。

### 页面的有效文件名

文件名由四个不同的部分组成：

1. 一个`数字` - 如果文件以数字作为前缀。
2. 一个分隔符 - 可以是`_`、`-`、空格或任意组合。
3. 一个`标签` - 是指除`.py`之外的所有内容。
4. 扩展名 - 总是`.py`。

### 侧边栏中显示的页面

侧边栏中显示的是文件名的`标签`部分：

- 如果没有`标签`，Streamlit将使用`数字`作为标签。
- 在用户界面中，Streamlit通过将`label`中的下划线替换为空格来美化标签。

### 侧边栏中页面的排序方式

排序会将文件名中的数字视为实际数字（整数）：

- 带有`number`的文件会排在没有`number`的文件之前。
- 文件的排序基于`number`（如果有），然后是`title`（如果有）。
- 在排序文件时，Streamlit将`number`视为实际的数字而不是字符串。因此`03`和`3`是相同的。

这个表格显示了文件名及其对应的标签的示例，按照它们在侧边栏中出现的顺序进行排序。

**示例**：

| **文件名**                 | **渲染标签**     |
| :------------------------ | :----------------- |
| `1 - first page.py`       | first page         |
| `12 monkeys.py`           | monkeys            |
| `123.py`                  | 123                |
| `123_hello_dear_world.py` | hello dear world   |
| `_12 monkeys.py`          | 12 monkeys         |

<提示>

可以使用表情符号使页面名称更有趣！例如，一个名为`🏠_Home.py`的文件将在侧边栏中创建一个名为"🏠 Home"的页面。

</提示>

## 在页面之间导航

页面会自动显示在应用程序的侧边栏中，以漂亮的导航界面展示。当您在侧边栏界面中点击一个页面时，Streamlit会导航到该页面，而不需要重新加载整个前端界面，使应用程序浏览速度极快！

您还可以使用URL在页面之间导航。每个页面都有自己的URL，由文件的`label`定义。当多个文件具有相同的`label`时，Streamlit会选择第一个文件（根据上述[描述的顺序](/library/get-started/multipage-apps#how-pages-are-sorted-in-the-sidebar)）。用户可以通过访问页面的URL来查看特定页面。

如果用户尝试访问一个不存在的页面的URL，他们将看到一个类似下面这样的模态框，提示用户请求的页面在应用的pages/目录中找不到。

<Image src="/images/mpa-page-not-found.png" />

## 注意事项

- 页面支持[魔法命令](https://docs.streamlit.io/library/api-reference/write-magic/magic)。
- 页面支持保存即运行。另外，当你保存一个页面时，这会导致正在查看该页面的用户重新运行页面。
- 添加或删除页面会立即更新用户界面。
- 在侧边栏更新页面不会重新运行脚本。
- `st.set_page_config` 在页面级别起作用。当您使用 [st.set_page_config](/library/api-reference/utilities/st.set_page_config) 设置标题或favicon时，仅适用于当前页面。
- 页面在全局范围内共享相同的Python模块：

  ```python
  # page1.py
  import foo
  foo.hello = 123

  # page2.py
  import foo
  st.write(foo.hello)  # 如果页面1已经执行，这应该输出123

- 页面共享相同的[st.session_state](https://docs.streamlit.io/library/advanced-features/session-state):

  ```python
  # page1.py
  import streamlit as st
  if "shared" not in st.session_state:
     st.session_state["shared"] = True

  # page2.py
  import streamlit as st
  st.write(st.session_state["shared"])
  # 如果页面1已经执行，这应该输出True
  ```

您现在对多页面应用有了扎实的理解。您已经学会了如何构建应用程序、定义页面并在用户界面之间导航。现在是时候[创建您的第一个多页面应用程序](/library/get-started/multipage-apps/create-a-multipage-app)了！🥳
