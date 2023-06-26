---
title: st.file_uploader 上传的文件存储在哪里？什么时候会被删除？
slug: /knowledge-base/using-streamlit/where-file-uploader-store-when-deleted
---

# st.file_uploader 上传的文件存储在哪里？什么时候会被删除？

当您使用 [`st.file_uploader`](/library/api-reference/widgets/st.file_uploader) 上传文件时，数据会通过浏览器复制到 Streamlit 后端，并以 BytesIO 缓冲区形式存储在 Python 内存中（即 RAM，而不是磁盘）。这些数据将在 Streamlit 应用程序从头到尾重新运行时持久保存在 RAM 中，即在每次小部件交互时。如果您需要在运行之间保存上传的数据，则可以使用 [缓存](/library/advanced-features/caching) 功能来使 Streamlit 在重新运行时持久保存它。

由于文件存储在内存中，一旦不再需要，它们就会立即被删除。

这意味着当以下情况发生时，Streamlit会将文件从内存中删除：

- 用户上传另一个文件，替换原始文件
- 用户清除文件上传器
- 用户关闭包含上传文件的浏览器标签页

相关论坛帖子：

- https://discuss.streamlit.io/t/streamlit-sharing-fileupload-where-does-it-go/9267
- https://discuss.streamlit.io/t/how-to-update-the-uploaded-file-using-file-uploader/13512/
