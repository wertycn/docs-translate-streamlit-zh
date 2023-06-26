---
标题：导入错误 libGL.so.1 无法打开共享对象文件：没有那个文件或目录
永久链接：/knowledge-base/dependencies/libgl

# 导入错误 libGL.so.1 无法打开共享对象文件：没有那个文件或目录

## 问题

在使用OpenCV在您部署在[Streamlit Community Cloud](https://streamlit.io/cloud)上的应用程序时，您收到错误消息`导入错误 libGL.so.1 无法打开共享对象文件：没有那个文件或目录`。

## 解决方案

如果您在应用程序中使用OpenCV，请在Streamlit Community Cloud的要求文件中包含`opencv-python-headless`，而不是`opencv_contrib_python`和`opencv-python`。

如果`opencv-python`是您的应用程序的_必需_（非可选）依赖项，或者是您的应用程序中使用的库的依赖项，则上述解决方案不适用。相反，您可以使用以下解决方案：

在您的代码库中创建一个名为 `packages.txt` 的文件，并在其中添加以下行，以安装 [apt-get 依赖](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#apt-get-dependencies) `libgl`：

```
libgl1
```
