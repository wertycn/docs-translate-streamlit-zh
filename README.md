---
{}
---

# Streamlit文档

[![Netlify状态](https://api.netlify.com/api/v1/badges/1ddc1b5a-ec21-4b66-987d-feeb68854c28/deploy-status?branch=main)](https://app.netlify.com/sites/streamlit-docs/deploys)

我们使用Next.js和Netlify来构建我们的[文档网站](https://docs.streamlit.io/)。

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

文档将在[http://localhost:3000](http://localhost:3000)上可见。请注意，每次修改`content/`文件夹中的源文件时，您需要刷新浏览器标签以查看更改。您**无需**重新启动开发服务器。

## 文件和文件夹结构

该存储库遵循典型的Next.js项目结构。要进行贡献，您只需编辑`content/`文件夹中的Markdown文件。

- `components/` 包含JS和MDX文件。
- `content/` 这是所有Markdown文件所在的文件夹。这是唯一需要编辑的文件夹。
- `lib/` 包含JS文件。
- `pages/` 您不需要编辑此文件夹。它包含处理复杂索引页面、URL片段映射和在`content/`中呈现Markdown页面的JSX文件。
- `public/` 包含我们文档中使用的所有图片。
- `python/` 包含用于生成API参考文档字符串的Python代码。
- `scripts/` 包含JS文件。
- `styles/` 包含用于样式和布局的CSS文件。

## 贡献

要在我们的文档中添加、编辑或删除内容，您需要修改`content/`文件夹及其子文件夹中的Markdown（.md）文件：

- `kb/` 包含填充知识库的.md文件。
- `library/` 包含填充Streamlit库部分的.md文件。
- `streamlit-cloud/` 包含填充Streamlit社区云部分的.md文件。
- `gdpr-banner.md` 您将永远不需要编辑此文件。
- `index.md` 包含填充索引页面的文本。
- `menu.md` 这是一个特殊文件，只包含定义文档菜单的前置内容。您需要在此文件中为您在文档站点中创建的每个新页面添加一个条目。

`content/`目录的结构并不重要，因为文件将被递归读取，并且会忽略目录。但是，我们建议按照目录结构组织文件，与文档网站上的结构相对应。

### 添加新页面

您想向文档中添加新页面吗？

1. 首先，决定页面应该位于哪个部分（Streamlit Library、Streamlit Community Cloud 或 Knowledge Base）。

2. 接下来，导航到`content/`文件夹和子文件夹中，并创建一个`.md`文件，文件名与页面标题相对应。例如，对于一个名为"创建组件"的页面，导航到`content/library/components/`并创建一个名为`create-component.md`的文件。

### `.md`文件的结构

现在，您已经决定了文件的位置并命名了文件，是时候添加内容了！

**文件格式**：

每个`.md`文件的顶部都有一个前置元数据，用于定义页面标题（显示在浏览器标签栏中）和URL slug（显示在`docs.streamlit.io/`和`localhost:3000/`之后的斜杠后面）。

例如，对于一个名为"创建组件"的页面，应该存在于`docs.streamlit.io/library/components/create`，`create-component.md`文件顶部的前置元数据如下：

```markdown
---
title: Create a Component
slug: /library/components/create
---
```

**标题:**

使用标准的Markdown语法来创建标题（#, ##, ###）

**Callouts（注释）:**

要添加一个注释（Note，Tip，Warning，Important），在适当的标签（MDX函数）中输入Markdown文本，确保在开标签之后添加一个空行，在闭标签之前再添加一个空行。例如：

```markdown
<Note>

This is a **note** that links to our [website](https://docs.streamlit.io/).

</Note>
```

**嵌入代码:**

将代码用 \` \` 包围起来以内嵌它。例如：

```markdown
Create a header with `st.header`.
```

像这样嵌入代码块：

````markdown
    ```python
    import streamlit as st

    st.text("Hello world")
    ```
````

我们支持对Python、Bash、TOML、SQL和JSX进行语法高亮显示。

**链接到其他文档页面:**

使用标准的Markdown语法将链接添加到文档中的其他页面。例如，通过包含在“创建应用程序”`.md`文件的front matter中定义的slug来添加一个内联链接到“创建应用程序”页面:

```markdown
Learn how to [Create an app](/library/get-started/create-an-app).
```

**添加图片：**

将要显示的图片存储在`/public/images/`目录下。有两种方式可以显示图片。

1. 要显示单张图片，可以使用常规的Markdown语法。确保图片路径以`/images/`开头，而不是`/public/images/`。例如：

   ```markdown
   ![Secrets manager screenshot](/images/databases/edit-secrets.png)
   ```

2. 要在同一行上显示多个图片，为每个图片添加一个包含alt文本和图片路径的`<Image />`标签，并将它们全部包含在`<Flex>`和`</Flex>`标签中。例如，要显示存储在`/public/images/databases/`中的3个图片：

   ```markdown
   <Flex>
   <Image alt="Bigquery截图7" src="/images/databases/big-query-7.png" />
   <Image alt="Bigquery截图8" src="/images/databases/big-query-8.png" />
   **可发现性:**

只需按照上述格式创建一个包含标题和slug的Markdown文件，即可在指定的URL上创建一个新页面。

但是，用户必须知道URL才能访问页面。因此，该页面是“可访问但不可发现的”。下一节将介绍如何将页面添加到文档菜单中，以便用户可以找到您的页面。

### 添加页面到菜单

如何让您创建的页面显示在菜单中？编辑特殊的markdown文件`content/menu.md`。该文件中只包含YAML格式的前置元数据。

假设您已经创建了一个名为"安装"的页面，该页面位于`docs.streamlit.io/library/get-started/installation`。您希望它显示在菜单中，位于"Streamlit Library"部分下的"Get Started"页面之下。

要这样做，请找到 `menu.md` 中定义 "Get Started" 的 `category`、`url` 和 `visible` 属性的行，并在其下方添加三行新的内容，包括：

```YAML
- category: Streamlit Library / Get Started / Installation
  url: /library/get-started/installation
  visible: true
```

> 重要提示：您**始终**需要将您创建的条目添加到`menu.md`中，否则构建将失败。这是重要的，因为我们使用此页面的结构来为每个页面创建面包屑导航。如果您不希望页面显示在菜单上，您仍然需要将其添加到`menu.md`中，但可以将其`visible`属性设置为`false`。

### 编辑现有页面

要编辑现有页面：

1. 找到页面的`.md`文件
2. 编辑Markdown并保存文件

刷新浏览器选项卡并访问编辑后的页面以预览您的更改！

### 向API参考中添加新的文档字符串

每次发布新版本的Streamlit时，都需要通过运行`make docstrings`来更新`python/streamlit.json`中存储的文档字符串。这将构建必要的Docker镜像，并使用最新发布的PyPi版本更新文件中的文档。

如果您需要重新生成所有版本的所有函数签名，请删除`python/streamlit.json`中的内容，但保留文件，并运行`make docstrings`命令。这将系统地安装每个版本的streamlit，并在`streamlit.json`中生成必要的函数签名。

假设一个新的Streamlit版本包含一个名为`st.my_chart`的函数，您想将其包含在API参考的"Chart elements"部分中：

1. 运行`make docstrings`命令。
2. 在`content/library/api/charts/`目录下创建Markdown文件(`my_chart.md`)
3. 在`my_chart.md`中添加以下内容:

   ```markdown
   ---
   title: st.my_chart
   slug: /library/api-reference/charts/st.my_chart
   ---

   <Autofunction function="streamlit.my_chart" />
   ```

4. 在`content/library/api/api-reference.md`文件的"Chart elements"标题下方添加以下内容:
   1. 包含在 `my_chart.md` 中定义的 URL slug 的 RefCard MDX 函数。这是出现在 API 参考首页上的卡片。
2. 包含 alt 文本和要显示在卡片上的图片位置的 Image MDX 函数。
3. 一个加粗的标题，将出现在卡片上 (`#### Heading`)。它出现在卡片图片下方。
4. 关于 `st.my_chart` 的简要描述。它出现在卡片标题下方。
   5. 以下是一个代码块示例，演示如何使用 `st.my_chart`。它将显示在卡片描述的下方，并且有一个复制图标，当用户点击时，将代码块复制到用户的剪贴板中。

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

5. 将以下两行新内容添加到`menu.md`中，以便在菜单中显示`st.my_chart`:

   ```YAML
   - category: Streamlit Library / API Reference / Chart elements / st.my_chart
     url: /library/api-reference/charts/st.my_chart
   ```

6. 保存更改并刷新浏览器选项卡。如果一切顺利，您应该在菜单中看到一个新的条目，在API参考中看到一个新的卡片，并且有一个关于`st.my_chart`的新页面。

### 添加到知识库

知识库（KB）分为五个部分：

1. **教程：** 逐步示例，演示如何在Streamlit中构建不同类型的应用程序
2. **使用Streamlit：** 关于使用Streamlit库的常见问题
3. **部署问题：** 关于部署Streamlit应用程序的文章
4. **Streamlit组件：** 关于Streamlit组件的文章
5. **安装依赖：** 在使用或部署Streamlit应用程序时，涉及系统和Python依赖问题的文章

如果您知道如何解决Streamlit用户的痛点并希望将其添加到知识库中：

1. 确定您的文章属于上述哪个部分
2. 导航到`kb/`目录中相应部分的文件夹
3. 在上述指定的格式中创建一个`.md`文件，其中包含您的文章

   - 确保正文中的标题和Markdown文件头部相同。例如：

     ```markdown
     ---
     title: 如何将组件添加到侧边栏？
     ```
     slug: /knowledge-base/components/add-component-sidebar
     ---

     # 如何将组件添加到侧边栏？

     ```
     
4. 在与您的文章相同的文件夹中的现有 `index.md` 文件中添加一行内容。该行应包含在文章的前置数据中指定的标题和URL slug。这一步骤确保用户能够在相关知识库部分的索引页面上找到您的文章。例如：

   ```markdown
   - [如何将组件添加到侧边栏？](/knowledge-base/components/add-component-sidebar)
   ```

## 发布

要发布您对文档站点的更改：

1. 创建一个包含您的更改的新分支。
2. 创建一个Pull Request，并将Snehan标记为审核人。
3. 等待检查完成后，切换到预览构建。
4. Snehan将审核您的更改并将其合并到`main`分支中。
5. 合并后，您的更改将在[https://docs.streamlit.io/](https://docs.streamlit.io/)上生效。