---
title: 发布组件
slug: /library/components/publish
---

# 发布组件

## 发布到 PyPI

将您的 Streamlit 组件发布到 [PyPI](https://pypi.org/) 可以使其在全球范围内轻松访问。这一步骤完全可选，所以如果您不打算公开发布组件，可以跳过本节！

<Note>

对于[静态Streamlit组件](/library/components/components-api#create-a-static-component)，将Python包发布到PyPI的步骤与[核心PyPI打包说明](https://packaging.python.org/tutorials/packaging-projects/)相同。静态组件可能只包含Python代码，因此一旦您的[setup.py](https://packaging.python.org/tutorials/packaging-projects/#creating-setup-py)文件正确无误，并且
[生成分发文件](https://packaging.python.org/tutorials/packaging-projects/#generating-distribution-archives)，您已经准备好[上传到PyPI](https://packaging.python.org/tutorials/packaging-projects/#uploading-the-distribution-archives)。

[双向Streamlit组件](/library/components/components-api#create-a-bi-directional-component)最少需要包含Python和JavaScript代码，因此在发布到PyPI之前需要进行一些准备工作。本页面的其余部分将重点介绍双向组件的准备过程。

</Note>

### 准备您的组件

一个双向的Streamlit组件与纯Python库略有不同，它必须包含预编译的前端代码。这也是基本的Streamlit工作原理；当你使用`pip install streamlit`安装时，你得到的是一个Python库，其中的HTML和前端代码已经编译为静态资源。

[component-template](https://github.com/streamlit/component-template) GitHub仓库提供了发布到PyPI所需的文件夹结构。但在发布之前，您需要进行一些清理工作：

1. 如果尚未命名，请为您的组件命名
   - 将`template/my_component/`文件夹重命名为`template/<组件名称>/`
   - 将组件名称作为`declare_component()`的第一个参数传递
2. 编辑 `MANIFEST.in`，将 `recursive-include` 的路径从 `package/frontend/build *` 修改为 `<component name>/frontend/build *`
3. 编辑 `setup.py`，添加您的组件名称和其他相关信息
4. 创建前端代码的发布构建。这将在 `frontend/build/` 目录中添加一个新的目录，其中包含您编译后的前端代码：

   ```bash
   cd frontend
   npm run build
   ```

5. 将构建文件夹的路径作为 `path` 参数传递给 `declare_component`。 (如果您使用的是模板 Python 文件，可以在文件顶部设置 `_RELEASE = True`)：

   ```python
      import streamlit.components.v1 as components

      # 将这行代码：
      # component = components.declare_component("my_component", url="http://localhost:3001")

      # 改为这行代码：
      parent_dir = os.path.dirname(os.path.abspath(__file__))
      build_dir = os.path.join(parent_dir, "frontend/build")
      component = components.declare_component("new_component_name", path=build_dir)
   ```

### 构建Python Wheel

在您修改了默认的`my_component`引用、编译了HTML和JavaScript代码，并在`components.declare_component()`中设置了新的组件名称后，您就可以构建一个Python wheel了：

1. 确保您拥有最新版本的setuptools、wheel和twine

2. 从源代码创建一个wheel包：

   ```bash
    # 从组件的顶层目录运行以下命令，即包含`setup.py`的目录
    python setup.py sdist bdist_wheel
   ```

### 将您的 wheel 上传到 PyPI

创建好 wheel 后，最后一步是将其上传到 PyPI。这里的说明重点介绍了如何上传到[Test PyPI](https://test.pypi.org/)，这样您就可以在不担心出错的情况下学习该过程的机制。上传到 PyPI 的过程遵循相同的基本步骤。

1. 如果您还没有Test PyPI的账户，请在[Test PyPI](https://test.pypi.org/)上创建一个账户。

   - 访问[https://test.pypi.org/account/register/](https://test.pypi.org/account/register/)并完成相应步骤。

   - 访问[https://test.pypi.org/manage/account/#api-tokens](https://test.pypi.org/manage/account/#api-tokens)，创建一个新的API令牌。不要将令牌范围限制在特定项目上，因为您正在创建一个新项目。在关闭页面之前，请复制您的令牌，因为您将无法再次检索它。

2. 将您的wheel上传到Test PyPI。`twine`将提示您输入用户名和密码。对于用户名，请使用**\_\_token\_\_**。对于密码，请使用上一步中的令牌值，包括`pypi-`前缀:

   ```bash
   python3 -m twine upload --repository testpypi dist/*
   ```

3. 在新的Python项目中安装您刚上传的包，以确保它可以正常工作:

   ```bash
    ```python
python -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-pkg-YOUR-USERNAME-HERE
```

如果一切顺利，您可以按照[https://packaging.python.org/tutorials/packaging-projects/#next-steps](https://packaging.python.org/tutorials/packaging-projects/#next-steps)中的说明将您的库上传到PyPI。

恭喜，您已经创建了一个公开可用的Streamlit组件！

推广您的组件！
```

我们非常乐意帮助您与Streamlit社区分享您的组件！要分享组件，请在[Streamlit 'Show the Community!'论坛分类](https://discuss.streamlit.io/c/streamlit-examples/9)上发布帖子，标题类似于 "新组件：`<您的组件名称>`，一种新的方式来做X"。

您还可以在Twitter上@streamlit发送推文，以便我们可以为您转发您的公告。

如果您将代码托管在GitHub上，请添加标签`streamlit-component`，以便它在[GitHub的**streamlit-component**主题](https://github.com/topics/streamlit-component)中列出：

<Image caption="将streamlit-component标签添加到您的GitHub存储库中" src="/images/component-tag.gif" />
