---
title: 组件
slug: /library/components
---

# 自定义组件

组件是第三方的Python模块，可以扩展Streamlit的功能。

## 如何使用组件

使用组件非常简单：

1. 首先找到您想要使用的组件。两个很好的资源是：

   - [组件库](https://streamlit.io/components)
   - [此帖子](https://discuss.streamlit.io/t/streamlit-components-community-tracker/4634)，
     由我们论坛的Fanilo A.提供。

2. 使用您喜欢的Python包管理器安装组件。此步骤及后续步骤在组件的说明中有详细描述。

   例如，要使用出色的[AgGrid
   组件](https://github.com/PablocFonseca/streamlit-aggrid)，您首先需要使用以下命令进行安装：

   ```python
   pip install streamlit-aggrid
   ```

3. 在您的Python代码中，按照说明导入组件。对于AgGrid来说，这一步是：

   ```python
   from st_aggrid import AgGrid
   ```

4. ...现在您已经可以使用它了！对于AgGrid，您可以这样使用：

   ```python
   AgGrid(my_dataframe)
   ```

## 制作自己的组件

如果您有兴趣制作自己的组件，请查看以下资源：

- [创建一个组件](/library/components/create)
- [发布一个组件](/library/components/publish)
- [组件 API](/library/components/components-api)
- [发布组件时的博客文章！](https://blog.streamlit.io/introducing-streamlit-components/)

另外，如果您更喜欢通过视频学习，我们的工程师Tim Conkling精心制作了一些令人惊叹的教程：

##### 视频教程，第一部分

<YouTube videoId="BuD3gILJW-Q" />

##### 视频教程，第二部分

<YouTube videoId="QjccJl_7Jco" />
