---
slug: /knowledge-base/using-streamlit/where-file-uploader-store-when-deleted
title: Where does st.file_uploader store uploaded files and when do they get deleted?
---

# st.file_uploader将上传的文件存储在哪里，它们何时被删除？

当您使用[`st.file_uploader`](/library/api-reference/widgets/st.file_uploader)上传文件时，数据会通过浏览器复制到Streamlit后端，并包含在Python内存（即RAM，而不是磁盘）中的BytesIO缓冲区中。数据将在RAM中持久存在，直到Streamlit应用程序从头到尾重新运行，即在每次小部件交互时。如果您需要在运行之间保存已上传的数据，则可以将其[缓存](/library/advanced-features/caching)，以便Streamlit在重新运行时保留数据。

由于文件存储在内存中，一旦不再需要，它们会立即被删除。

这意味着当以下情况发生时，Streamlit会从内存中删除文件：

- 用户上传另一个文件，替换原始文件
- 用户清除文件上传器
- 用户关闭上传文件的浏览器选项卡

相关论坛帖子：

- https://discuss.streamlit.io/t/streamlit-sharing-fileupload-where-does-it-go/9267
- [如何使用文件上传器更新已上传的文件](https://discuss.streamlit.io/t/how-to-update-the-uploaded-file-using-file-uploader/13512/)