---
title: 如何在Streamlit Community Cloud上增加st.file_uploader的上传限制？
slug: /knowledge-base/deploy/increase-file-uploader-limit-streamlit-cloud
---

# 如何在Streamlit Community Cloud上增加st.file_uploader的上传限制？

## 概述

默认情况下，使用[`st.file_uploader()`](/library/api-reference/widgets/st.file_uploader)上传的文件限制为200MB。您可以通过配置`server.maxUploadSize`选项来更改这个限制。

Streamlit提供了设置配置选项的四种不同方式：

1. 在**全局配置文件**中，macOS/Linux为`~/.streamlit/config.toml`，Windows为`%userprofile%/.streamlit/config.toml`：
   ```toml
   [server]
   maxUploadSize = 200
   ```
2. 在**项目配置文件**中，位于`$CWD/.streamlit/config.toml`，其中`$CWD`是您运行Streamlit的文件夹路径。
3. 通过`STREAMLIT_*`环境变量，例如:
   ```bash
   export STREAMLIT_SERVER_MAX_UPLOAD_SIZE=200
   ```
4. 在运行`streamlit run`时作为命令行的标志:
   ```bash
   streamlit run your_script.py --server.maxUploadSize 200
   ```

对于部署在[Streamlit Community Cloud](/streamlit-community-cloud)上的应用程序，您应该选择哪个选项呢？🤔

## 解决方案

当将应用程序部署到Streamlit Community Cloud时，您应该**使用选项1**。即，在全局配置文件（`.streamlit/config.toml`）中设置`maxUploadSize`配置选项，然后将其上传到您的应用程序的GitHub仓库中。🎈

例如，要将上传限制增加到400MB，请将以下行内容的`.streamlit/config.toml`文件上传到您的应用程序的GitHub仓库中：

```toml
[server]
maxUploadSize = 400
```

## 相关资源

- [Streamlit拖放限制在200MB，需要解决方法](https://discuss.streamlit.io/t/streamlit-drag-and-drop-capping-at-200mb-need-workaround/19803/2)
- [文件上传小部件API](/library/api-reference/widgets/st.file_uploader)
- [如何设置Streamlit配置选项](/library/advanced-features/configuration#set-configuration-options)
