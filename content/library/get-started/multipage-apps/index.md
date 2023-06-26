---
slug: /library/get-started/multipage-apps
title: Multipage apps
---

# 多页面应用

随着应用程序变得越来越大，将其组织为多个页面变得非常有用。这样可以使开发人员更轻松地管理应用程序，用户也更容易导航。Streamlit提供了一种无缝创建多页面应用的方式。页面会自动显示在应用程序侧边栏的一个漂亮的导航小部件中，点击页面将在不重新加载前端的情况下导航到该页面，使应用程序浏览速度极快！

在[上一页](/library/get-started/create-an-app)中，我们创建了一个"单页应用"，用于探索纽约市的Uber公共数据集中的接送点。在本指南中，让我们学习如何创建多页面应用程序。一旦我们对创建多页面应用程序的要求有了坚实的基础，让我们在[下一节](/library/get-started/multipage-apps/create-a-multipage-app)中自己构建一个!

## 结构化多页面应用程序

让我们了解创建多页面应用所需的步骤，包括如何定义页面、组织和运行多页面应用以及在用户界面中导航页面。一旦您了解了基础知识，您可以直接转到[下一节](/library/get-started/multipage-apps/create-a-multipage-app)，将熟悉的`streamlit hello`命令转换为多页面应用！

## 运行多页面应用

运行多页面应用与运行单页面应用相同。运行多页面应用的命令是：

```python
streamlit run [entrypoint file]
```

"入口文件"是应用程序向用户展示的第一页。一旦您在应用程序中添加了页面，入口文件将出现在侧边栏的顶部。您可以将入口文件视为应用程序的"主页面"。例如，假设您的入口文件是`Home.py`。然后，要运行您的应用程序，您可以运行`streamlit run Home.py`。这将启动您的应用程序并执行`Home.py`中的代码。

## 添加页面

创建入口文件后，您可以通过在相对于入口文件的`pages/`目录中创建`.py`文件来添加页面。例如，如果您的入口文件是`Home.py`，那么您可以创建一个`pages/About.py`文件来定义"关于"页面。下面是一个多页面应用程序的有效目录结构示例:

```
Home.py # This is the file you run with "streamlit run"
└─── pages/
  └─── About.py # This is a page
  └─── 2_Page_two.py # This is another page
  └─── 3_😎_three.py # So is this
```

<提示>

在给文件名添加表情符号时，最佳实践是包含一个带有编号前缀，以便在终端中进行自动补全更加方便。终端的自动补全功能可能会对Unicode（表情符号的表示方式）感到困惑。

</提示>

页面被定义为`pages/`目录中的`.py`文件。页面的文件名根据[下面的规则](#how-pages-are-labeled-and-sorted-in-the-ui)转换为侧边栏中的页面名称。例如，`About.py`文件将在侧边栏中显示为"About"，`2_Page_two.py`将显示为"Page two"，`3_😎_three.py`将显示为“😎 three”：

![目录结构](/images/mpa-add-pages.png)

只有 `pages/` 目录中的 `.py` 文件才会被加载为页面。Streamlit 忽略 `pages/` 目录和子目录中的所有其他文件。

## 页面在用户界面中的标签和排序方式

侧边栏用户界面中的页面标签是根据文件名生成的。它们可能与 [`st.set_page_config`](/library/api-reference/utilities/st.set_page_config) 中设置的页面标题不同。让我们来了解一下什么样的文件名是一个有效的页面文件名，页面在侧边栏中如何显示，以及页面的排序方式。

### 页面的有效文件名

文件名由四个不同的部分组成：

1. `数字` - 如果文件以数字开头。
2. 分隔符 - 可以是 `_`，`-`，空格或其任意组合。
3. `标签` - 这是文件名中`.py`之前的部分，但不包括`.py`。
4. 扩展名 - 总是`.py`。

### 侧边栏中显示的页面

在侧边栏中显示的是文件名的`标签`部分：

- 如果没有`标签`，Streamlit将使用`数字`作为标签。
- 在用户界面中，Streamlit通过将`label`中的下划线替换为空格来美化显示。

### 侧边栏中页面的排序方式

排序方式会将文件名中的数字视为实际的数字（整数）：

- 具有`number`的文件会排在没有`number`的文件之前。
- 文件根据`number`（如果有）和`title`（如果有）进行排序。
- 在排序文件时，Streamlit将`number`视为实际的数字，而不是字符串。所以`03`和`3`是相同的。

这个表格展示了文件名和对应标签的示例，按照它们在侧边栏中出现的顺序进行排序。

**示例**：

| **文件名**                | **显示标签**     |
| :------------------------ | :----------------- |
| `1 - first page.py`       | first page         |
| `12 monkeys.py`           | monkeys            |
| `123.py`                  | 123                |
| `123_hello_dear_world.py` | hello dear world   |
| `_12 monkeys.py`          | 12 monkeys         |

<Tip>

可以使用表情符号使页面名称更有趣！例如，一个名为`🏠_Home.py`的文件将在侧边栏中创建一个标题为"🏠 Home"的页面。

</Tip>

## 在页面之间导航

页面会自动显示在应用程序侧边栏的漂亮导航界面中。当您在侧边栏界面中点击一个页面时，Streamlit会导航到该页面，而不会重新加载整个前端，使应用程序浏览速度非常快！

您也可以使用URL在页面之间进行导航。每个页面都有自己的URL，由文件的`label`定义。当多个文件具有相同的`label`时，Streamlit会选择第一个文件（基于上面描述的排序方式）。用户可以通过访问页面的URL来查看特定页面。

如果用户尝试访问一个不存在的页面的URL，他们将看到一个类似下面这样的模态框，提示用户请求的页面在应用程序的pages/目录中找不到。

![页面未找到](/images/mpa-page-not-found.png)

## 注意事项

- 页面支持[魔法命令](https://docs.streamlit.io/library/api-reference/write-magic/magic)。
- 页面支持保存即运行。此外，当您保存一个页面时，这会导致当前正在查看该页面的用户重新运行该页面。
- 添加或删除页面会立即更新UI。
- 在侧边栏中更新页面不会重新运行脚本。
- `st.set_page_config` 在页面级别上起作用。当您使用 [st.set_page_config](/library/api-reference/utilities/st.set_page_config) 设置标题或网站图标时，这仅适用于当前页面。
- 页面在全局范围内共享相同的Python模块：

  ```python
  # page1.py
  import foo
  foo.hello = 123

  # page2.py
  import foo
  st.write(foo.hello)  # 如果已经执行了page1，这应该输出123

- 页面共享相同的[st.session_state](https://docs.streamlit.io/library/advanced-features/session-state):

  ```python
  # page1.py
  import streamlit as st
  if "shared" not in st.session_state:
     st.session_state["shared"] = True

  # page2.py
  import streamlit as st
  st.write(st.session_state["shared"])
  # 如果已经执行了page1，这应该输出True
  ```

您现在对多页面应用有了扎实的了解。您已经学会了如何构建应用程序，定义页面，并在用户界面之间进行导航。现在是时候[创建您的第一个多页面应用程序](/library/get-started/multipage-apps/create-a-multipage-app)了！🥳