# Streamlit文档

[![Netlify状态](https://api.netlify.com/api/v1/badges/1ddc1b5a-ec21-4b66-987d-feeb68854c28/deploy-status?branch=main)](https://app.netlify.com/sites/streamlit-docs/deploys)

我们使用Next.js和Netlify构建我们的[文档网站](https://docs.streamlit.io/)。

## 构建

要构建文档，请克隆此存储库，安装NPM依赖项，并启动开发服务器。

1. 克隆此存储库：

```bash
git clone https://github.com/streamlit/docs.git
cd docs/
```

2. 安装NPM依赖项

```bash
make
```

3. 启动开发服务器：

```bash
make up
```

文档可以在 [http://localhost:3000](http://localhost:3000) 上查看。请注意，每当您修改 `content/` 文件夹中的源文件时，您需要刷新浏览器标签以查看更改。您**不需要**重新启动开发服务器。

## 文件和文件夹结构

该存储库遵循典型的 Next.js 项目结构。要贡献内容，您只需编辑 `content/` 文件夹中的 Markdown 文件。

- `components/` 包含 JS 和 MDX 文件。
- `content/` 这是存放所有Markdown文件的文件夹，这是唯一需要编辑的文件夹。
- `lib/` 包含JS文件。
- `pages/` 您不需要编辑此文件夹。它包含处理复杂的索引页面、URL标识映射和在`content/`中呈现Markdown页面的JSX文件。
- `public/` 包含我们文档中使用的所有图片。
- `python/` 包含生成API参考文档字符串的Python代码。
- `scripts/` 包含JS文件。
- `styles/` 包含用于样式和布局的CSS文件。

## 贡献

要在我们的文档中添加、编辑或删除内容，您需要修改`content/`文件夹及其子文件夹中的Markdown（`.md`）文件：

- `kb/` 包含用于填充知识库的`.md`文件。
- `library/` 包含用于填充Streamlit库部分的`.md`文件。
- `streamlit-cloud/` 包含用于填充Streamlit社区云部分的`.md`文件。
- `gdpr-banner.md` 您将永远不需要编辑此文件。
- `index.md` 包含填充索引页面的文本。
- `menu.md` 这是一个特殊的文件，只包含定义文档菜单的前置内容。您需要为在文档站点中创建的每个新页面在此文件中添加条目。

`content/`目录的结构并不重要，因为文件将被递归读取，并忽略目录。但是，我们建议按照文档网站上的结构来组织文件，以反映其结构。

### 添加新页面

您想要向文档中添加新页面吗？

1. 首先，决定页面应该放在哪个部分（Streamlit库、Streamlit社区云还是知识库）。

2. 接下来，导航到`content/`目录下的相关文件夹和子文件夹，并创建一个`.md`文件，文件名与页面标题相同。例如，对于一个名为"创建组件"的页面，导航到`content/library/components/`并创建一个名为`create-component.md`的文件。

### `.md`文件的结构

现在，您已经确定了文件的位置并命名了文件，是时候添加内容了！

**文件格式**：

每个`.md`文件的顶部都有一个前置元数据，用于定义页面标题（显示在浏览器标签栏中）和URL slug（显示在`docs.streamlit.io/`和`localhost:3000/`后面的斜杠之后）。

例如，对于一个标题为"创建组件"的页面，应该存在于`docs.streamlit.io/library/components/create`，在`create-component.md`文件的顶部的前置元数据如下所示：

```markdown
---
title: Create a Component
slug: /library/components/create
---
```

**标题：**

使用标准的Markdown语法来设置标题（#、##、###）

**Callouts（提示）：**

要添加一个提示（注意、提示、警告、重要），请在相应的标签（MDX函数）中输入Markdown文本，确保在开标签后和闭标签前分别添加一个空行。例如：

```markdown
<Note>

This is a **note** that links to our [website](https://docs.streamlit.io/).

</Note>
```

**嵌入代码:**

使用 \` \` 将代码嵌入到行内。例如：

```markdown
Create a header with `st.header`.
```

嵌入代码块的方法如下所示：

````markdown
    ```python
    import streamlit as st

    st.text("Hello world")
    ```
````

我们支持Python、Bash、TOML、SQL和JSX的语法高亮显示。

**链接到文档中的其他页面:**

使用标准的Markdown语法来链接到文档中的其他页面。例如，通过在“Create an app”`.md`文件的正文中包含在前置元数据中定义的slug，可以添加一个内联链接到“Create an app”页面。

```markdown
Learn how to [Create an app](/library/get-started/create-an-app).
```

**添加图片：**

将要显示的图片存储在`/public/images/`目录中。有两种方法可以显示图片。

1. 要显示单个图片，可以使用普通的Markdown语法。确保图片路径从`/images/`开始，而不是`/public/images/`。例如：

   ```markdown
   ![Secrets manager screenshot](/images/databases/edit-secrets.png)
   ```

2. 要在同一行上显示多个图像，为每个图像添加一个包含alt文本和图像路径的`<Image />`标签，并将它们都包裹在`<Flex>` `</Flex>`标签内。例如，要显示存储在`/public/images/databases/`目录下的3个图像：

   ```markdown
   <Flex>
   <Image alt="Bigquery截图7" src="/images/databases/big-query-7.png" />
   <Image alt="Bigquery截图8" src="/images/databases/big-query-8.png" />
   ![Bigquery screenshot 9](/images/databases/big-query-9.png)

</Flex>
```

**可发现性：**

只需按照上述描述的格式创建一个包含标题和slug的Markdown文件，即可在URL上提供一个新页面。

然而，用户必须知道URL才能访问该页面。因此，该页面是"可达到但不可发现"的。下一节将介绍如何将页面添加到文档菜单中，以便用户可以找到您的页面。

### 将页面添加到菜单

如何让您创建的页面显示在菜单中？编辑特殊的Markdown文件 `content/menu.md`。它只包含YAML格式的前置内容。

假设您已经创建了一个名为"Installation"的页面，可在 `docs.streamlit.io/library/get-started/installation` 上找到。您希望它显示在菜单中的"Streamlit Library"部分下，嵌套在"Get Started"页面下方。

要这样做，请找到`menu.md`中定义“开始”页面的`category`、`url`和`visible`属性的行，并在其下方添加三行新内容，包括：

```YAML
- category: Streamlit Library / Get Started / Installation
  url: /library/get-started/installation
  visible: true
```

> 重要提示: 您**必须**在`menu.md`中添加您创建的条目，否则构建将失败。这很重要，因为我们使用此页面的结构为每个页面创建面包屑导航。如果您不希望页面显示在菜单中，您仍然需要添加它，但是您可以将其`visible`属性设置为`false`。

### 编辑现有页面

要编辑现有页面，请按照以下步骤进行操作:

1. 找到页面的`.md`文件
2. 编辑Markdown内容并保存文件

要预览您的更改，请刷新浏览器选项卡并访问编辑后的页面！

### 向API参考添加新的文档字符串

每次发布新版本的Streamlit时，都需要通过运行`make docstrings`来更新存储在`python/streamlit.json`中的文档字符串。这将构建必要的Docker镜像，并使用最新发布的PyPi版本更新文件中的文档。

如果您需要重新生成所有版本的所有函数签名，请删除`python/streamlit.json`中的内容，保留文件，并运行`make docstrings`。这将按顺序安装每个版本的streamlit，并在`streamlit.json`中生成所需的函数签名。

假设新的Streamlit发布包含一个名为`st.my_chart`的函数，您想将其包含在API参考的"Chart elements"部分中：

1. 运行`make docstrings`
2. 在`content/library/api/charts/`目录下创建Markdown文件(`my_chart.md`)
3. 将以下内容添加到`my_chart.md`文件中:

   ```markdown
   ---
   title: st.my_chart
   slug: /library/api-reference/charts/st.my_chart
   ---

   <Autofunction function="streamlit.my_chart" />
   ```

4. 在`content/library/api/api-reference.md`文件的"Chart elements"标题下添加以下内容:
   1. `my_chart.md` 中定义的 URL slug 的 RefCard MDX 函数。这是显示在 API 参考首页上的卡片。
2. 包含 alt 文本和要显示在卡片上的图像位置的 Image MDX 函数。
3. 一个粗体标题，将显示在卡片上 (`#### 标题`)。它出现在卡片图像下方。
4. `st.my_chart` 的简要描述。它出现在卡片标题下方。
   5. 展示如何使用 `st.my_chart` 的代码块。它出现在卡片描述的下方，并且有一个复制图标，点击该图标可以将代码块复制到用户的剪贴板中。

````markdown
    <RefCard href="/library/api-reference/charts/st.my_chart">
    <Image pure alt="Tux, the Linux mascot" src="/img/data-table.png" />

    #### My charts

    Display a chart using the MyChart library.

    ```python
    st.my_chart(my_data_frame)
    ```

    </RefCard>
````

5. 在`menu.md`文件中添加以下2行，以便`st.my_chart`出现在菜单中:

   ```YAML
   - category: Streamlit Library / API Reference / Chart elements / st.my_chart
     url: /library/api-reference/charts/st.my_chart
   ```

6. 保存更改并刷新浏览器标签。如果一切顺利，您应该在菜单中看到一个新的条目，在API参考中看到一个新的卡片，并且有一个关于`st.my_chart`的新页面。

### 添加到知识库

知识库（KB）分为五个部分：

1. **教程：** 逐步示例，展示如何在Streamlit中构建不同类型的应用程序
2. **使用Streamlit：** 关于使用Streamlit库的常见问题
3. **部署问题：** 关于部署Streamlit应用程序的文章
4. **Streamlit组件：** 关于Streamlit组件的文章
5. **安装依赖：** 使用或部署Streamlit应用程序时涉及的系统和Python依赖问题

如果您知道如何解决Streamlit用户的问题，并希望将其添加到知识库中:

1. 确定您的文章属于上述哪个部分
2. 导航到`kb/`中相关部分的文件夹
3. 创建一个以指定格式命名的`.md`文件，其中包含您的文章

   - 确保正文中的标题和Markdown文件头部的标题相同。例如:

     ```markdown
     ---
     title: 如何将组件添加到侧边栏?
     ```
     slug: /knowledge-base/components/add-component-sidebar
     ---

     # 如何将组件添加到侧边栏？

     1. 在与文章相同的文件夹中的现有`index.md`文件中添加一行。它应包含在文章的front matter中指定的标题和URL slug。这一步确保用户能够在相关知识库部分的索引页面上发现您的文章。例如：

   ```markdown
   - [如何将组件添加到侧边栏？](/knowledge-base/components/add-component-sidebar)
   ```

## 发布

要将更改发布到文档站点，请按照以下步骤进行操作：

1. 创建一个包含您的更改的新分支。
2. 创建一个拉取请求，并将Snehan标记为审阅者。
3. 等待检查完成后，切换到预览构建。
4. Snehan将审查您的更改并将其合并到`main`分支中。
5. 合并后，您的更改将在[https://docs.streamlit.io/](https://docs.streamlit.io/)上生效。
