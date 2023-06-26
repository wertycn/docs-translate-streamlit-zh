---
slug: /knowledge-base/deploy/increase-file-uploader-limit-streamlit-cloud
title: How do I increase the upload limit of st.file_uploader on Streamlit Community
  Cloud?
---

# 如何增加Streamlit社区云上的st.file_uploader的上传限制？

## 概述

默认情况下，使用[`st.file_uploader()`](/library/api-reference/widgets/st.file_uploader)上传的文件限制为200MB。您可以使用`server.maxUploadSize`配置选项进行配置。

Streamlit提供了[四种不同的设置配置选项的方式](/library/advanced-features/configuration#set-configuration-options)：

1. 在 macOS/Linux 系统下的全局配置文件 `~/.streamlit/config.toml` 或 Windows 系统下的 `%userprofile%/.streamlit/config.toml` 中：
   ```toml
   [server]
   maxUploadSize = 200
   ```

2. 在项目配置文件 `$CWD/.streamlit/config.toml` 中，其中 `$CWD` 是您运行 Streamlit 的文件夹。

3. 通过 `STREAMLIT_*` 环境变量进行配置，例如：
   ```bash
   export STREAMLIT_SERVER_MAX_UPLOAD_SIZE=200
   ```
4. 在运行`streamlit run`命令时，作为命令行标志：
   ```bash
   streamlit run your_script.py --server.maxUploadSize 200
   ```

如果您将应用程序部署到[Streamlit Community Cloud](/streamlit-community-cloud)，那么应该选择这四个选项中的哪一个呢？🤔

## 解决方案

在将您的应用程序部署到Streamlit Community Cloud时，您应该**使用选项1**。也就是，在一个全局配置文件（`.streamlit/config.toml`）中设置`maxUploadSize`配置选项，并将其上传到您的应用程序的GitHub存储库中。 🎈

例如，如果要将上传限制增加到400MB，可以将包含以下内容的`.streamlit/config.toml`文件上传到您的应用程序的GitHub存储库中：

```toml
[server]
maxUploadSize = 400
```

## 相关资源

- [Streamlit拖放上传文件限制在200MB，需要解决方案](https://discuss.streamlit.io/t/streamlit-drag-and-drop-capping-at-200mb-need-workaround/19803/2)
- [文件上传小部件API](/library/api-reference/widgets/st.file_uploader)
- [如何设置Streamlit配置选项](/library/advanced-features/configuration#set-configuration-options)