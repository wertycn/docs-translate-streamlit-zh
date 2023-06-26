---
slug: /knowledge-base/dependencies/libgl
title: ImportError libGL.so.1 cannot open shared object file No such file or directory
---

# ImportError libGL.so.1无法打开共享对象文件：没有那个文件或目录

## 问题

在使用OpenCV部署在[Streamlit社区云](https://streamlit.io/cloud)上的应用程序时，您会收到错误消息`ImportError libGL.so.1无法打开共享对象文件：没有那个文件或目录`。

## 解决方案

如果您在应用程序中使用了OpenCV，请在Streamlit社区云的要求文件中将`opencv_contrib_python`和`opencv-python`替换为`opencv-python-headless`。

如果 `opencv-python` 是您的应用程序的 _必需_ (非可选) 依赖项，或者是您的应用程序中使用的库的依赖项，上述解决方案不适用。相反，您可以使用以下解决方案：

在您的存储库中创建一个名为 `packages.txt` 的文件，并添加以下行以安装 [apt-get 依赖项](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#apt-get-dependencies) `libgl`：

```
libgl1
```