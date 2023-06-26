---
slug: /library/components/publish
title: Publish a Component
---

# 发布组件

## 发布到 PyPI

将您的 Streamlit 组件发布到 [PyPI](https://pypi.org/) 可以让全世界的 Python 用户轻松访问。这一步是完全可选的，如果您不打算公开发布您的组件，可以跳过这个部分！

<注意>

对于[静态 Streamlit 组件](/library/components/components-api#create-a-static-component)，发布 Python 包到 PyPI 的步骤与以下步骤相同：
[核心PyPI打包说明](https://packaging.python.org/tutorials/packaging-projects/)。静态组件可能只包含Python代码，因此一旦您的[setup.py](https://packaging.python.org/tutorials/packaging-projects/#creating-setup-py)文件正确并且[生成了分发文件](https://packaging.python.org/tutorials/packaging-projects/#generating-distribution-archives)，您就准备好了。
[上传到PyPI](https://packaging.python.org/tutorials/packaging-projects/#uploading-the-distribution-archives)。

双向Streamlit组件（Bi-directional Streamlit Components）至少包括Python和JavaScript代码，因此在发布到PyPI之前需要进行一些准备工作。本页面的剩余部分将重点介绍双向组件的准备过程。

</Note>

### 准备您的组件

双向的 Streamlit 组件与纯 Python 库略有不同，它必须包含预编译的前端代码。这也是基本的 Streamlit 的工作原理；当你使用 `pip install streamlit` 安装时，你将得到一个包含在其中的 HTML 和前端代码已经被编译为静态资源的 Python 库。

[component-template](https://github.com/streamlit/component-template) GitHub仓库提供了PyPI发布所需的文件夹结构。但在发布之前，您需要进行一些清理工作：

1. 给您的组件取一个名称（如果还没有）
   - 将`template/my_component/`文件夹重命名为`template/<component name>/`
   - 将您的组件名称作为`declare_component()`的第一个参数传递
2. 编辑 `MANIFEST.in`，将 `recursive-include` 的路径从 `package/frontend/build *` 修改为 `<component name>/frontend/build *`
3. 编辑 `setup.py`，添加您的组件名称和其他相关信息
4. 创建前端代码的发布版本。这将在 `frontend/build/` 目录下添加一个新的目录，其中包含编译后的前端代码:

   ```bash
   cd frontend
   npm run build
   ```

5. 将构建文件夹的路径作为`path`参数传递给`declare_component`函数。（如果您使用的是模板Python文件，可以在文件顶部设置`_RELEASE = True`）：

   ```python
      import streamlit.components.v1 as components

      # 将下面这行代码：
      # component = components.declare_component("my_component", url="http://localhost:3001")

      # 替换为下面这行代码：
      parent_dir = os.path.dirname(os.path.abspath(__file__))
      build_dir = os.path.join(parent_dir, "frontend/build")
      component = components.declare_component("new_component_name", path=build_dir)
   ```

### 构建Python wheel

一旦您更改了默认的`my_component`引用，编译了HTML和JavaScript代码，并在`components.declare_component()`中设置了您的新组件名称，您就可以开始构建Python wheel：

1. 确保您安装了最新版本的setuptools、wheel和twine

2. 从源代码创建一个wheel包：

   ```bash
    # 在组件的顶级目录（即包含`setup.py`的目录）中运行以下命令：
    python setup.py sdist bdist_wheel
   ```

### 将你的Wheel上传到PyPI

创建好你的Wheel后，最后一步是将其上传到PyPI。这里的说明介绍了如何上传到[Test PyPI](https://test.pypi.org/)，这样你可以学习整个过程的机制，而不用担心弄乱任何东西。上传到PyPI的基本流程相同。

1. 如果您还没有账户，请在[Test PyPI](https://test.pypi.org/)上创建一个账户。

   - 访问[https://test.pypi.org/account/register/](https://test.pypi.org/account/register/)并按照步骤完成注册。

   - 访问[https://test.pypi.org/manage/account/#api-tokens](https://test.pypi.org/manage/account/#api-tokens)，并创建一个新的API令牌。不要将令牌范围限制在特定项目上，因为您正在创建一个新项目。在关闭页面之前，请复制您的令牌，因为您将无法再次检索它。

2. 将您的wheel上传到Test PyPI。`twine`会提示您输入用户名和密码。对于用户名，请使用**\_\_token\_\_**。对于密码，请使用上一步中的令牌值，包括`pypi-`前缀：

   ```bash
   python3 -m twine upload --repository testpypi dist/*
   ```

3. 在一个新的Python项目中安装您新上传的包以确保其正常工作：

   ```bash
    ```python
python -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-pkg-YOUR-USERNAME-HERE
```

如果一切顺利，您就可以按照[https://packaging.python.org/tutorials/packaging-projects/#next-steps](https://packaging.python.org/tutorials/packaging-projects/#next-steps)上的说明将您的库上传到PyPI。

恭喜您，您已经创建了一个公开可用的Streamlit组件！

## 推广您的组件！
```

我们非常乐意帮助您与Streamlit社区分享您的组件！要分享它，请在[Streamlit 'Show the Community!'论坛分类](https://discuss.streamlit.io/c/streamlit-examples/9)上发布标题类似于"New Component: `<your component name>`, a new way to do X"的帖子。

您还可以在Twitter上@streamlit我们，我们会为您转发您的公告。

如果您将代码托管在GitHub上，请添加`streamlit-component`标签，以便它会被列在[GitHub的**streamlit-component**主题](https://github.com/topics/streamlit-component)中：

<Image caption="将streamlit-component标签添加到您的GitHub仓库" src="/images/component-tag.gif" />