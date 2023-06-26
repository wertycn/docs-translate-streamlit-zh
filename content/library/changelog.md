---
slug: /library/changelog
title: Changelog
---

# 更新日志

本页面列出了官方Streamlit发布版本的主要亮点、错误修复和已知问题。如果您想了解夜间版本、测试版功能或实验性功能的信息，请参阅[尝试预发布功能](/library/advanced-features/prerelease)。

<提示>

要升级到最新版本的Streamlit，请运行：

```bash
pip install --upgrade streamlit
```

</Tip>

## **版本 1.23.0**

发布日期：2023 年 6 月 1 日

**亮点**

- ✂️ 宣布 [st.data_editor](/library/api-reference/data/st.data_editor) 的正式可用，这是一个允许您在表格样式的用户界面中编辑 DataFrame 和许多其他数据结构的小部件。**重大变更：**在 `st.session_state` 中使用的数据编辑器的表示形式已经改变。了解更多关于新格式的信息，请参阅 [访问编辑后的数据](/library/advanced-features/dataframes#access-edited-data)。
- ⚙️ 介绍了[Column配置API](/library/api-reference/data/st.column_config)，提供了一系列方法来配置`st.dataframe`和`st.data_editor`列的显示和编辑行为（例如标题、可见性、类型或格式）。请密切关注在接下来的两周内发布的详细[博客文章](https://blog.streamlit.io/)和深入的[文档](/library/advanced-features/dataframes#configuring-columns)。
- 🔌 学习使用 `st.experimental_connection` 在您的应用程序中创建和管理数据连接，详细说明请参考新的[连接到数据](/library/advanced-features/connecting-to-data)文档和[视频教程](https://www.youtube.com/watch?v=xQwDfW7UHMo)。

**重要变更**

- 📊 Streamlit现在支持Protobuf 4和Altair 5 ([#6215](https://github.com/streamlit/streamlit/issues/6215), [#6618](https://github.com/streamlit/streamlit/pull/6618), [#5626](https://github.com/streamlit/streamlit/issues/5626), [#6622](https://github.com/streamlit/streamlit/pull/6622)).
- ☎️ `st.dataframe` 和 `st.data_editor` 可以使用 `hide_index` 参数隐藏索引列，使用 `column_order` 参数指定列的显示顺序，并使用 `disabled` 参数禁用单独的列的编辑功能。
- ⏱️ 在 [st.cache_data](/library/api-reference/performance/st.cache_data) 和 [st.cache_resource](/library/api-reference/performance/st.cache_resource) 中的 `ttl` 参数接受格式化的字符串，所以您可以简单地使用 `ttl="30d"`、`ttl="1h30m"` 或者其他由 [Pandas 的 Timedelta 构造器](https://pandas.pydata.org/docs/reference/api/pandas.Timedelta.html) 支持的 `w`、`d`、`h`、`m`、`s` 组合来指定时间间隔。([#6560](https://github.com/streamlit/streamlit/pull/6560))。
- 📂 `st.file_uploader` 现在更准确地解释 `type` 参数。例如，现在 "jpg" 或 ".jpg" 都可以接受 "jpg" 和 "jpeg" 扩展名。此功能还扩展到了 "mpeg/mpg"、"tiff/tif"、"html/htm" 和 "mpeg4/mp4"。
- 🤫 新的 `global.disableWidgetStateDuplicationWarning` 配置选项允许通过同时设置小部件默认值和键入会话状态值来静音警告（[#3605](https://github.com/streamlit/streamlit/issues/3605), [#6640](https://github.com/streamlit/streamlit/pull/6640)）。感谢 [@antonAce](https://github.com/antonAce)！

**其他变更**

- 🏃‍♀️通过延迟加载一些依赖项改进了启动时间（[#6531](https://github.com/streamlit/streamlit/pull/6531)）。
- 👋由于弃用和低使用率，移除了 `st.beta_*` 和 `st.experimental_show`（[#6558](https://github.com/streamlit/streamlit/pull/6558)）
- 🚀对 st.dataframe 和 st.data_editor 进一步改进：
  - 对数据编辑器在移动设备上进行了改进（[#6548](https://github.com/streamlit/streamlit/pull/6548)）。
  - 所有可编辑的列在列头部都有一个图标，并支持工具提示（[#6550](https://github.com/streamlit/streamlit/pull/6550), [#6561](https://github.com/streamlit/streamlit/pull/6561)）。
- 对包含日期时间、日期或时间值的列启用编辑功能（[#6025](https://github.com/streamlit/streamlit/pull/6025)）。
  - 数据编辑器中的列现在支持新的输入验证选项，例如文本列的`max_chars`和`validate`，以及数字列的`min_value`、`max_value`和`step`（[#6563](https://github.com/streamlit/streamlit/pull/6563)）。
  - 数据编辑器中的类型解析能力得到了改进（[#6551](https://github.com/streamlit/streamlit/pull/6551)）。
  - 返回的数据结构中的缺失值统一为`None`（[#6544](https://github.com/streamlit/streamlit/pull/6544)）。
  - 当整数超过最大安全值`(2^53) -1`时，单元格会显示警告信息（[#6311](https://github.com/streamlit/streamlit/issues/6311), [#6549](https://github.com/streamlit/streamlit/pull/6549)）。
  - 通过显示警告信息来防止编辑会话状态（[#6634](https://github.com/streamlit/streamlit/pull/6634)）。
  - 修复了列表列有时会破坏前端的问题（[#6644](https://github.com/streamlit/streamlit/pull/6644)）。
  - 修复了使用类别类型的索引列显示问题（[#6680](https://github.com/streamlit/streamlit/issues/6680)，[#6598](https://github.com/streamlit/streamlit/pull/6598)）。
  - 修复了添加空行时无法重新运行的问题（[#6598](https://github.com/streamlit/streamlit/pull/6598)）。
  - 统一了`st.data_editor`和`st.dataframe`的行为，根据输入数据自动隐藏索引列（[#6659](https://github.com/streamlit/streamlit/issues/6659)，[#6598](https://github.com/streamlit/streamlit/pull/6598)）
- 🛡️ Streamlit的[安全策略](https://github.com/streamlit/streamlit/blob/develop/SECURITY.md)可以在其GitHub存储库中找到（[#6666](https://github.com/streamlit/streamlit/pull/6666)）。
- 🤏对`st.number_input`和`st.slider`的整数大小限制进行了文档说明([#6724](https://github.com/streamlit/streamlit/pull/6724))。
- 🐍 Streamlit的大部分Python依赖项都设置了最大允许版本，标准的上限设置为下一个主要版本，但不包含在内([#6691](https://github.com/streamlit/streamlit/pull/6691))。
- 💅 改进了应用内模态框的UI设计([#6688](https://github.com/streamlit/streamlit/pull/6688))。
- 🐞 错误修复：`st.date_input`在深色模式下日期选择器可见性相同 ([#6072](https://github.com/streamlit/streamlit/issues/6072)，[#6630](https://github.com/streamlit/streamlit/pull/6630))。
- 🐜 错误修复：多页面应用程序中的侧边栏导航扩展指示符已恢复 ([#6731](https://github.com/streamlit/streamlit/pull/6731))。
- 🐛 Bug修复：更新了`st.set_page_config`的文档字符串和异常消息，以澄清该命令可以在多页面应用程序中的每个页面上调用一次，而不是每个应用程序调用一次（[#6594](https://github.com/streamlit/streamlit/pull/6594)）。
- 🐝 修复了`st.json`在渲染时不再将键和值中的多个空格合并为单个空格的问题（[#6657](https://github.com/streamlit/streamlit/issues/6657), [#6663](https://github.com/streamlit/streamlit/pull/6663))。

## **版本 1.22.0**

_发布日期：2023 年 4 月 27 日_

**亮点**

- 🔌 引入 `st.experimental_connection`：使用我们的新连接功能，轻松将您的应用程序连接到数据源和 API。在[API 参考](/library/api-reference/connections)中查找更多详细信息，并关注即将发布的博客文章和深入文档！同时，您可以参考我们更新后的 [MySQL](/knowledge-base/tutorials/databases/mysql) 和 [Snowflake](/knowledge-base/tutorials/databases/snowflake) 连接教程，了解该功能的示例。

**重要变更**

- 🐼 Streamlit 现在支持 Pandas 2.0 ([#6413](https://github.com/streamlit/streamlit/issues/6413), [#6378](https://github.com/streamlit/streamlit/pull/6378), [#6507](https://github.com/streamlit/streamlit/pull/6507))。感谢 [connortann](https://github.com/connortann)！
- 🍔 使用`client.toolbarMode` [配置选项](https://docs.streamlit.io/library/advanced-features/configuration#view-all-configuration-options)（[#6174](https://github.com/streamlit/streamlit/pull/6174)）自定义工具栏、选项菜单和设置对话框中项目的可见性。
- 🪵 Streamlit日志现在存储在“streamlit”命名空间中，而不是根记录器，使应用程序开发人员能够更好地管理日志处理（[#3978](https://github.com/streamlit/streamlit/issues/3978)，[#6377](https://github.com/streamlit/streamlit/pull/6377)）。

**其他变更**

- 🔏 CLI参数不再用于设置敏感配置值（[#6376](https://github.com/streamlit/streamlit/pull/6376)）。
- 🤖 通过减少日志噪音来改善调试体验（[#6391](https://github.com/streamlit/streamlit/pull/6391)）。
- 🐞 错误修复：`@st.cache_data` 装饰的函数支持 UUID 对象作为参数（[#6440](https://github.com/streamlit/streamlit/issues/6440)，[#6459](https://github.com/streamlit/streamlit/pull/6459)）。
- 🐛 修复：现在通过标签切换按钮和其他元素时，只有在焦点在其上时才显示红色边框，而不是在点击时也显示红色边框（[#6373](https://github.com/streamlit/streamlit/pull/6373)）。
- 🪲 修复：`st.multiselect`的清除图标更大，并包含悬停效果（[#6471](https://github.com/streamlit/streamlit/pull/6471)）。
- 🐜 修复：自定义主题字体设置不再应用于代码块 ([#6484](https://github.com/streamlit/streamlit/issues/6484), [#6535](https://github.com/streamlit/streamlit/pull/6535)).
- ©️ 修复：`st.code`的复制到剪贴板按钮在悬停在代码块上时出现 ([#6490](https://github.com/streamlit/streamlit/issues/6490), [#6498](https://github.com/streamlit/streamlit/pull/6498)).

## **版本 1.21.0**

发布日期：2023年4月6日

**亮点**

- 📏 介绍 `st.divider` 命令，该命令可在您的应用程序中显示一条水平线。在其 [API 参考](/library/api-reference/text/st.divider) 中了解如何使用此命令。
- 🔏 Streamlit 现在支持使用全局的 `secrets.toml` 文件，除了项目级文件之外，以便轻松存储和安全访问您的机密信息。在 [机密管理](/library/advanced-features/secrets-management) 中了解更多信息。
- 🚀 [st.help](/library/api-reference/utilities/st.help) 已经进行了改进，可以展示更多关于对象方法、属性、类等的信息，这对于调试非常有帮助（[#5857](https://github.com/streamlit/streamlit/pull/5857), [#6382](https://github.com/streamlit/streamlit/pull/6382))！

**重要变更**

- 🪜 [st.time_input](/library/api-reference/widgets/st.time_input) 支持使用关键字参数 `step` 添加步进间隔 ([#6071](https://github.com/streamlit/streamlit/pull/6071)).
- ❓ 大多数 [文本元素](/library/api-reference/text) 可以通过 `help` 参数添加工具提示 ([#6043](https://github.com/streamlit/streamlit/pull/6043)).
- ↔️ [st.pyplot](/library/api-reference/charts/st.pyplot)具有一个`use_container_width`参数，可以将图表设置为容器的宽度（现在所有的[图表元素](/library/api-reference/charts)都支持这个参数）（[#6067](https://github.com/streamlit/streamlit/pull/6067)）。
- 👩‍💻 [st.code](/library/api-reference/text/st.code) 支持通过布尔参数 `line_numbers`（[#5756](https://github.com/streamlit/streamlit/issues/5756), [#6042](https://github.com/streamlit/streamlit/pull/6042)）可选择在代码块左侧显示行号。
- 通过设置 `anchor=False`（[#6158](https://github.com/streamlit/streamlit/pull/6158)），可以关闭标题元素中的锚点功能。

**其他变更**

- 🐼 [st.table](/library/api-reference/data/st.table) 和 [st.dataframe](/library/api-reference/data/st.dataframe) 支持 `pandas.Period`、数字和布尔类型的分类列（[#2547](https://github.com/streamlit/streamlit/issues/2547)、[#5429](https://github.com/streamlit/streamlit/pull/5429)、[#5329](https://github.com/streamlit/streamlit/issues/5392)、[#6248](https://github.com/streamlit/streamlit/pull/6248))。
- 🕸️ 将`.webp`添加到允许的静态文件扩展名列表中（[#6331](https://github.com/streamlit/streamlit/pull/6331)）
- 🐞 Bug修复：在WebSocket关闭时停止脚本执行以立即清除会话信息（[#6166](https://github.com/streamlit/streamlit/issues/6166)，[#6204](https://github.com/streamlit/streamlit/pull/6204))。
- 🐜 错误修复：更新了允许/不允许标签的 Markdown 行为，使得不支持的元素被解除包装，只有它们的子元素（文本内容）被渲染（[#5872](https://github.com/streamlit/streamlit/issues/5872), [#6036](https://github.com/streamlit/streamlit/issues/6036), [#6054](https://github.com/streamlit/streamlit/issues/6054), [#6163](https://github.com/streamlit/streamlit/pull/6163))。
- 🪲 修复了一些错误：在重新运行时不会推送浏览器历史状态，使用HTTPS加载`streamlit hello`中的外部资源，并使浏览器的后退按钮在多页面应用程序中正常工作（[#5292](https://github.com/streamlit/streamlit/issues/5292), [#6266](https://github.com/streamlit/streamlit/pull/6266), [#6232](https://github.com/streamlit/streamlit/pull/6232)）。感谢 [whitphx](https://github.com/whitphx)！
- 🐝 错误修复：避免在非UTF-8终端显示表情符号。([#2284](https://github.com/streamlit/streamlit/issues/2284), [#6088](https://github.com/streamlit/streamlit/pull/6088))。感谢[kcarnold](https://github.com/kcarnold)的贡献！
- 📁 修复Bug：覆盖了`react-dropzone`对于[File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API)的默认使用，使得`st.file_uploader`的文件选择对话框只显示与`type`参数中包含的文件类型对应的文件（[#6176](https://github.com/streamlit/streamlit/issues/6176), [#6315](https://github.com/streamlit/streamlit/pull/6315)）。
- 💾 Bug fix: 修复了缓存修饰函数上的`.clear()`方法无法工作的问题（[#6310](https://github.com/streamlit/streamlit/issues/6310), [#6321](https://github.com/streamlit/streamlit/pull/6321)）。
- 🏃 Bug fix: `st.experimental_get_query_params`不需要重新运行即可工作（[#6347](https://github.com/streamlit/streamlit/issues/6347), [#6348](https://github.com/streamlit/streamlit/pull/6348)）。感谢[PaleNeutron](https://github.com/PaleNeutron)!
- 🐛 Bug修复：`CachedStFunctionWarning`提到了已弃用的`suppress_st_warning`而不是`experimental_allow_widgets`（[#6216](https://github.com/streamlit/streamlit/issues/6216)，[#6217](https://github.com/streamlit/streamlit/pull/6217))。

## **版本 1.20.0**

_发布日期：2023年3月9日_

**重要变更**

- 🔐 增加了对配置SSL的支持，以便[直接通过HTTPS提供应用程序](/library/advanced-features/https-support)（[#5969](https://github.com/streamlit/streamlit/pull/5969)）。
- 🖼️ 使用 `/?embed` 和 `/?embed_options` 查询参数对应用程序的嵌入行为进行细粒度控制。了解如何使用此功能，请参阅我们的[文档](/streamlit-community-cloud/get-started/deploy-an-app#embed-apps) ([#6011](https://github.com/streamlit/streamlit/pull/6011), [#6019](https://github.com/streamlit/streamlit/pull/6019))。
- ⚡ 将`runner.fastReruns` [配置选项](/library/advanced-features/configuration#view-all-configuration-options) 默认启用，以使应用程序对用户交互更加响应 ([#6200](https://github.com/streamlit/streamlit/pull/6200))。

**其他更改**

- 🍔 通过删除最少使用的选项来简化汉堡菜单 ([#6080](https://github.com/streamlit/streamlit/pull/6080))。
- 🖨️ 对应用程序进行设计更改，以确保打印或保存为PDF时的显示效果良好（[#6180](https://github.com/streamlit/streamlit/pull/6180)）。
- 🐞 修复了 `st.experimental_data_editor` 中 `dtypes` 检查的问题（[#6185](https://github.com/streamlit/streamlit/issues/6185), [#6188](https://github.com/streamlit/streamlit/pull/6188)）。
- 🐛 修复了在不在列中时，正确定位 `st.metric` 的 `help` 工具提示的问题（[#6168](https://github.com/streamlit/streamlit/pull/6168)）。
- 🪲 修复错误：修复了从服务器的`ForwardMsgCache`中检索消息时的回归问题（[#6210](https://github.com/streamlit/streamlit/pull/6210)）。
- 🌀 修复错误：`st.cache_data`的文档字符串现在在`show_spinner`参数中列出了`str`作为支持的类型（[#6207](https://github.com/streamlit/streamlit/issues/6207)，[#6213](https://github.com/streamlit/streamlit/pull/6213)）。
- ⏱️ 更宽容地设置了ping和websocket的超时时间（[#6212](https://github.com/streamlit/streamlit/pull/6212)）。
- 🗺️ `st.map`和`st.pydeck_chart`文档中指出Streamlit的Mapbox令牌将不会无限制地工作（[#6143](https://github.com/streamlit/streamlit/pull/6143)）。

## **版本1.19.0**

_发布日期：2023年2月23日_

**亮点**

- ✂️介绍 `st.experimental_data_editor`，一个允许您在类似表格的用户界面中编辑DataFrame和许多其他数据结构的小部件。在我们的[文档](/library/advanced-features/dataframes)和[博客文章](https://blog.streamlit.io/editable-dataframes-are-here/)中了解更多信息。

**其他变更**

- ✨ Streamlit的GitHub README页面有了新的外观([#6016](https://github.com/streamlit/streamlit/pull/6016)).
- 🌚 改进了在暗模式下样式化数据帧单元格的可读性（[#6060](https://github.com/streamlit/streamlit/issues/6060), [#6098](https://github.com/streamlit/streamlit/pull/6098)).
- 🐛 修复错误：使应用在最新版本的Safari中正常工作，并在禁用第三方Cookie的Chrome中正常工作（[#6092](https://github.com/streamlit/streamlit/issues/6092)，[#6094](https://github.com/streamlit/streamlit/pull/6094)，[#6087](https://github.com/streamlit/streamlit/issues/6087)，[#6100](https://github.com/streamlit/streamlit/pull/6100)).
- 🐞 修复错误：在“清除缓存”对话框和错误消息中引用新的缓存原语（[#6082](https://github.com/streamlit/streamlit/pull/6082)，[#6128](https://github.com/streamlit/streamlit/pull/6128)）。
- 🐝 修复错误：正确地缓存类成员函数和实例方法（[#6109](https://github.com/streamlit/streamlit/issues/6109)，[#6114](https://github.com/streamlit/streamlit/pull/6114)）。
- 🐜 修复：`st.metric` 工具提示位置的回归问题（[#6093](https://github.com/streamlit/streamlit/issues/6093)，[#6129](https://github.com/streamlit/streamlit/pull/6129)）。
- 🪲 修复：允许数据框、图表等在展开器中显示全屏按钮（[#6083](https://github.com/streamlit/streamlit/pull/6083)，[#6148](https://github.com/streamlit/streamlit/pull/6148)）。

## **版本 1.18.0**

发布日期：2023 年 2 月 9 日

**亮点**

- 🎊 介绍`@st.cache_data`和`@st.cache_resource`两个新的缓存命令来替代`st.cache`！请查看我们的[博客文章](https://blog.streamlit.io/p/c0a90231-9848-47ec-a40c-ad4a344e4de1/)和[文档](/library/advanced-features/caching)获取更多信息。

**重要变更**

- 🪆 `st.columns`在应用程序的主区域支持最多一级列嵌套（即，列内嵌套列）。
- ⏳ `st.progress`函数支持使用`text`关键字参数在进度条上方显示消息。
- ↔️ `st.button`函数有一个可选的`use_container_width`参数，可以让按钮在整个容器宽度上拉伸。
- 🐍 我们正式支持Python 3.11。
- 🖨️ 通过应用程序汉堡菜单中的“打印”选项，将您的应用程序保存为PDF文件。
- 🛎️ 应用程序可以通过`enableStaticServing`配置选项来提供小型的静态媒体文件。请参阅我们的[文档](/library/advanced-features/static-file-serving)了解如何使用此功能，并查看我们的演示[应用程序](https://static-file-serving.streamlit.app/)以获取示例。

**其他变更**

- 🏁 所有的Streamlit端点（包括`/healthz`）都已经重命名，以保持一致的模式，并避免与GCP（尤其是Cloud Run和App Engine）的保留端点发生冲突（[#5534](https://github.com/streamlit/streamlit/pull/5534)）。
- ⚡ 当多个会话同时访问一个未计算的缓存值时，改进了缓存性能（[#6017](https://github.com/streamlit/streamlit/pull/6017)）。
- 🚧 当`client.showErrorDetails`配置选项设置为`True`时，Streamlit仅在浏览器中显示废弃警告。无论是否在浏览器中显示，废弃警告都会被记录在控制台中（[#5945](https://github.com/streamlit/streamlit/pull/5945)）。
- 🏓 重构了`st.dataframe`的内部，以改进对数据框的处理和转换，例如检测更多类型、将键值字典转换为数据框等（[#6026](https://github.com/streamlit/streamlit/pull/6026)，[#6023](https://github.com/streamlit/streamlit/pull/6023)）。
- 💽 当传递不支持的Markdown元素时，小部件标签的行为已经有了文档记录（[#5978](https://github.com/streamlit/streamlit/pull/5978)）。
- 📊 错误修复：Plotly改进 - 升级多个前端依赖项，包括Plotly，到最新版本，以正确重新绘制缓存的图表，使Plotly地图动画正常工作，并允许用户在使用Streamlit主题时更新图表布局（[#5885](https://github.com/streamlit/streamlit/pull/5885)，[#5967](https://github.com/streamlit/streamlit/pull/5967)，[#6055](https://github.com/streamlit/streamlit/pull/6055))。
- 📶 错误修复: 允许浏览器标签在短暂断开连接时（由于网络故障、负载均衡器超时等）避免丢失所有状态 ([#5856](https://github.com/streamlit/streamlit/pull/5856)).
- 📱 错误修复: 当 `st.selectbox` 和 `st.multiselect` 的选项少于10个时，移动端隐藏键盘 ([#5979](https://github.com/streamlit/streamlit/pull/5979)).
- 🐝 修复了 `st.metric`、`st.multiselect`、`st.tabs` 的设计调整以及菜单项，以防止标签溢出和滚动问题，特别是在小视口大小下（[#5933](https://github.com/streamlit/streamlit/pull/5933), [#6034](https://github.com/streamlit/streamlit/pull/6034)）。
- 🐞 修复了在 `st.set_page_config` 中加载页面图标的 Twemoji URL 无效的问题（[#5943](https://github.com/streamlit/streamlit/pull/5943)）。
- ✍️ 增加了更多类型提示 ([#5986](https://github.com/streamlit/streamlit/pull/5986))。感谢 [harahu](https://github.com/harahu)！

## **版本 1.17.0**

发布日期：2023年1月12日

**重要变更**

- 🪄 [`@st.experimental_singleton`](/library/api-reference/performance/st.experimental_singleton#validating-the-cache) 支持可选的 `validate` 参数，接受一个验证函数来验证缓存的数据，并在每次访问缓存值时调用该函数。
- 💾 [`@st.experimental_memo`](https://docs.streamlit.io/library/api-reference/performance/st.experimental_memo)的`persist`参数也可以接受布尔值。

**其他更改**

- 📟 多页面应用程序将`__init__.py`从页面选择器中排除（[#5890](https://github.com/streamlit/streamlit/pull/5890)）。
- 📐 嵌入应用程序的iframe可以动态调整其高度（[#5894](https://github.com/streamlit/streamlit/pull/5894)）。
- 🐞 修复：范围滑块的缩略值将 respec 容器宽度 ([#5913](https://github.com/streamlit/streamlit/pull/5913)).
- 🪲 修复：Streamlit 命令的所有示例注释中包含相关的导入，以使它们可复现 ([#5877](https://github.com/streamlit/streamlit/pull/5877))。

## **版本 1.16.0**

发布日期：2022 年 12 月 14 日

**亮点**

- 👩‍🎨 为Altair、Plotly和Vega-Lite图表引入了一个新的Streamlit主题！请查看我们的[博客文章](https://blog.streamlit.io/a-new-streamlit-theme-for-altair-and-plotly/)获取更多信息。
- 🎨 Streamlit现在在所有接受Markdown的命令中支持彩色文本，包括`st.markdown`、`st.header`等。请在我们的[文档](/library/api-reference/text/st.markdown)中了解更多。

**重要更改**

- 🔁 使用 `st.experimental_memo` 或 `st.experimental_singleton` 缓存的函数可以包含 Streamlit 的媒体元素和表单。
- ⛄ 所有接受 pandas DataFrames 作为输入的 Streamlit 命令也支持 Snowpark 和 PySpark DataFrames。
- 🏷 [st.checkbox](/library/api-reference/widgets/st.checkbox) 和 [st.metric](/library/api-reference/data/st.metric) 可以使用 `label_visibility` 参数自定义如何隐藏它们的标签。

**其他变更**

- 🗺️ `st.map`改进：支持大写列名和更好的异常消息（[#5679](https://github.com/streamlit/streamlit/pull/5679)，[#5792](https://github.com/streamlit/streamlit/pull/5792)）。
- 🐞 Bug修复：`st.plotly_chart`遵循图表的高度属性和`use_container_width`参数（[#5779](https://github.com/streamlit/streamlit/pull/5779)）。
- 🪲 错误修复：包含`icon`参数的所有命令，如[st.error](/library/api-reference/status/st.error)，[st.warning](/library/api-reference/status/st.warning)等，都可以包含带有变体选择器的表情符号([#5583](https://github.com/streamlit/streamlit/pull/5583)).
- 🐝 错误修复：防止在调整浏览器窗口大小时`st.camera_input`抖动([#5661](https://github.com/streamlit/streamlit/pull/5711)).
- 🐜 Bug修复：更新异常布局以避免堆栈跟踪溢出（[#5700](https://github.com/streamlit/streamlit/pull/5700)）。

## **版本 1.15.0**

发布日期：2022 年 11 月 17 日

**重要变更**

- 💅 小部件标签可以包含内联的Markdown。请参阅我们的[文档](https://docs.streamlit.io/library/api-reference/widgets)和演示[应用程序](https://markdown-labels.streamlit.app/)获取更多信息。
- 🎵 [`st.audio`](/library/api-reference/media/st.audio) 现在支持通过关键字参数`sample_rate`将音频数据作为NumPy数组传递并播放。
- 🔁 使用`st.experimental_memo`或`st.experimental_singleton`缓存的函数可以包含使用`experimental_allow_widgets`参数的Streamlit小部件。这允许缓存复选框、滑块、单选按钮等等！

**其他变更**

- 👩‍🎨 对滑块进行设计调整以防止抖动（[#5612](https://github.com/streamlit/streamlit/pull/5612)）。
- 🐛 修复错误：标题中的链接为红色而不是蓝色（[#5609](https://github.com/streamlit/streamlit/pull/5609)）。
- 🐞 修复错误：在退出全屏模式时正确调整Plotly图表的大小（[#5645](https://github.com/streamlit/streamlit/pull/5645)）。
- 🐝 修复错误：不要意外触发`st.balloons`和`st.snow`（[#5401](https://github.com/streamlit/streamlit/pull/5401)）。

## **版本 1.14.0**

发布日期：2022 年 10 月 27 日

**亮点**

- 🎨 `st.button` 和 `st.form_submit_button` 支持使用 `type` 关键字参数将按钮标记为“主要”（以增强重点）或“次要”（普通按钮）。

**重要更改**

- 🤏 `st.multiselect` 增加了一个关键字参数 `max_selections`，用于限制每次可选择的选项数量。
- 📄 `st.form_submit_button` 现在具有 `disabled` 参数，可以去除互动性。

**其他变更**

- 🏓 `st.dataframe` 和 `st.table` 接受分类间隔作为输入（[#5395](https://github.com/streamlit/streamlit/pull/5395)）。
- ⚡ 对 Plotly 图表进行性能优化（[#5542](https://github.com/streamlit/streamlit/pull/5542)）。
- 🪲 修复了 `st.download_button` 在文件名中支持非拉丁字符的问题（[#5465](https://github.com/streamlit/streamlit/pull/5465)）。
- 🐞 错误修复：允许`st.image`将本地GIF渲染为GIF，而不是静态PNG（[#5438](https://github.com/streamlit/streamlit/pull/5438)）。
- 📱 多页面应用程序中侧边栏的设计微调（[#5538](https://github.com/streamlit/streamlit/pull/5538)，[#5445](https://github.com/streamlit/streamlit/pull/5445)，[#5559](https://github.com/streamlit/streamlit/pull/5559)）。
- 📊 内置图表的轴配置改进（[#5412](https://github.com/streamlit/streamlit/pull/5412)）。
- 🔧 便笺和单例改进：支持`show_spinner`的文本值，将`datetime.timedelta`对象用作`ttl`参数值，正确哈希PIL图像和`Enum`类，在返回未评估的数据帧时显示更好的错误消息（[#5447](https://github.com/streamlit/streamlit/pull/5447), [#5413](https://github.com/streamlit/streamlit/pull/5413), [#5504](https://github.com/streamlit/streamlit/pull/5504), [#5426](https://github.com/streamlit/streamlit/pull/5426), [#5515](https://github.com/streamlit/streamlit/pull/5515))。
- 🔍 使用 `st.map` 和 `st.pydeck_chart` 创建的地图中的缩放按钮根据应用程序的主题使用浅色或深色样式（[#5479](https://github.com/streamlit/streamlit/pull/5479)）。
- 🗜 可以通过新的 "internal"（即：可能会在不受弃用影响的情况下更改）API 获取当前会话的传入 WebSocket 请求的 WebSocket 标头（[#5457](https://github.com/streamlit/streamlit/pull/5457)）。
- 📝 改进了Streamlit在首次安装和使用时打印的文本（[#5473](https://github.com/streamlit/streamlit/pull/5473))。

## **版本1.13.0**

发布日期：2022年9月22日

**重要变更**

- 🏷 控件可以使用`label_visibility`参数自定义标签的隐藏方式。
- 🔍 `st.map` 默认添加缩放按钮到地图上。
- ↔️ `st.dataframe` 支持`use_container_width`参数，可以自动调整宽度以适应整个容器。
- 🪄 对 `st.dataframe` 的尺寸进行了改进：列宽计算将考虑列标题，支持在列标题之间双击进行自动调整大小，更好的全屏支持，并修复了 `width` 参数的问题。

**其他更改**

- ⌨️ `st.time_input` 允许使用键盘进行输入 ([#5194](https://github.com/streamlit/streamlit/pull/5194)).
- 💿 `st.memo`在使用`ttl`和`persist`关键字参数时会警告用户 ([#5032](https://github.com/streamlit/streamlit/pull/5032)).
- 🔢 `st.number_input`在重新运行后返回一致的类型 ([#5359](https://github.com/streamlit/streamlit/pull/5359)).
- 🚒 `st.sidebar` UI修复，包括修复Firefox浏览器中的滚动条问题 ([#5157](https://github.com/streamlit/streamlit/pull/5157), [#5324](https://github.com/streamlit/streamlit/pull/5324)).
- 👩‍💻 改进使用度量以指导 API 开发。
- ✍️ 更多的类型提示！([#5191](https://github.com/streamlit/streamlit/pull/5191), [#5192](https://github.com/streamlit/streamlit/pull/5192), [#5242](https://github.com/streamlit/streamlit/pull/5242), [#5243](https://github.com/streamlit/streamlit/pull/5243), [#5244](https://github.com/streamlit/streamlit/pull/5244), [#5245](https://github.com/streamlit/streamlit/pull/5245), [#5246](https://github.com/streamlit/streamlit/pull/5246)) 感谢 [harahu](https://github.com/harahu)！

## **版本 1.12.0**

_发布日期：2022 年 8 月 11 日_

**亮点**

- 📊 内置图表（例如 `st.line_chart`）拥有全新的外观和参数 `x` 和 `y`！请查看我们的[博客文章](https://blog.streamlit.io/built-in-charts-get-a-new-look-and-parameters/)获取更多信息。

**重要更改**

- ⏯ 使用 `st.experimental_memo` 或 `st.experimental_singleton` 进行缓存的函数现在可以包含静态 `st` 命令。这使得可以缓存文本、图表、数据框等内容！
- ↔️ 侧边栏现在可以通过拖放调整大小。
- ☎️ `st.info`、`st.success`、`st.error` 和 `st.warning` 进行了重新设计，并且添加了一个新的关键字参数：`icon`。

**其他更改**

- 🎚️ `st.select_slider` 现在正确处理所有浮点数（[#4973](https://github.com/streamlit/streamlit/pull/4973)，[#4978](https://github.com/streamlit/streamlit/pull/4978)）。
- 🔢 `st.multi_select` 现在可以接受枚举类型的值（[#4987](https://github.com/streamlit/streamlit/pull/4987)）。
- 🍊 `st.slider` 现在可以通过 `st.session_state` 设置范围值（[#5007](https://github.com/streamlit/streamlit/pull/5007)）。
- 🎨 `st.progress` 进行了重新设计（[#5011](https://github.com/streamlit/streamlit/pull/5011)，[#5086](https://github.com/streamlit/streamlit/pull/5086)）。
- 🔘 `st.radio` 更好地处理类似列表的数据框（[#5021](https://github.com/streamlit/streamlit/pull/5021)）。
- 🧞‍♂️ `st.cache` 现在能够正确处理 JSON 文件（[#5023](https://github.com/streamlit/streamlit/pull/5023)）。
- ⚓️ 当设置了 `anchor` 参数时，标题现在可以渲染 Markdown（[#5038](https://github.com/streamlit/streamlit/pull/5038)）。
- 🗻 `st.image` 现在可以从 Inkscape 加载 SVG 文件（[#5040](https://github.com/streamlit/streamlit/pull/5040)）。
- 🗺️ `st.map`和`st.pydeck_chart`根据应用程序的主题使用浅色或深色风格（[#5074](https://github.com/streamlit/streamlit/pull/5074)，[#5108](https://github.com/streamlit/streamlit/pull/5108)）。
- 🎈 点击`st.balloons`和`st.snow`下方的元素不再被阻止（[#5098](https://github.com/streamlit/streamlit/pull/5098)）。
- 🔝 嵌入式应用程序的顶部填充较小（[#5111](https://github.com/streamlit/streamlit/pull/5111)）。
- 💅 调整了小部件、图表和数据框的填充和对齐方式 ([#4995](https://github.com/streamlit/streamlit/pull/4995), [#5061](https://github.com/streamlit/streamlit/pull/5061), [#5081](https://github.com/streamlit/streamlit/pull/5081)).
- ✍️ 添加了更多的类型提示! ([#4926](https://github.com/streamlit/streamlit/pull/4926), [#4932](https://github.com/streamlit/streamlit/pull/4932), [#4933](https://github.com/streamlit/streamlit/pull/4933))

## **版本 1.11.0**

_发布日期：2022年7月14日_

**亮点**

- 🗂 引入 `st.tabs` 来在您的应用中创建选项卡容器。请参阅我们的[文档](/library/api-reference/layout/st.tabs)了解如何使用此功能。

**重要更改**

- ℹ️ `st.metric` 支持使用 `help` 关键字参数显示工具提示。
- 🚇 `st.columns` 支持使用 `gap` 关键字参数设置列之间的间隔大小。

**其他更改**

- 💅 对 `st.selectbox`、`st.expander`、`st.spinner` 进行了设计调整 ([#4801](https://github.com/streamlit/streamlit/pull/4801))
- 📱 在移动设备上，当用户从导航菜单中选择页面时，侧边栏将关闭 ([#4851](https://github.com/streamlit/streamlit/pull/4841))
- 🧠 `st.memo` 支持数据类！([#4850](https://github.com/streamlit/streamlit/pull/4850))
- 🏎 修复了一种竞态条件，该条件会在快速交互时破坏小部件状态（[#4882](https://github.com/streamlit/streamlit/pull/4882)）。
- 🏓 `st.table` 在放置在列和展开器内时，可以滚动显示溢出的内容（[#4934](https://github.com/streamlit/streamlit/pull/4934)）。
- 🐍 类型：在 Streamlit 中进行了更多的类型注解更新！（[#4808](https://github.com/streamlit/streamlit/pull/4808)，[#4809](https://github.com/streamlit/streamlit/pull/4809)，[#4856](https://github.com/streamlit/streamlit/pull/4856))

## **版本 1.10.0**

发布日期：2022 年 6 月 2 日

**亮点**

- 📖 引入了对多页面应用的原生支持！请查看我们的[博客文章](https://blog.streamlit.io/introducing-multipage-apps)并尝试我们的新 `streamlit hello`。

**主要变更**

- ✨ `st.dataframe` 已经重新设计。
- 🔘 `st.radio` 现在具有一个 `horizontal` 关键字参数，可以水平显示选项。
- ⚠️ Streamlit Community Cloud 支持更丰富的异常格式化。
- 🏂 使用 `st.experimental_user` 可以在私有应用程序中获取用户信息。

**其他变更**

- 📊 升级了Vega-Lite库，以支持更多的交互式图表改进。请查看它们的[发布说明](https://github.com/vega/vega-lite/releases)以了解更多信息。([#4751](https://github.com/streamlit/streamlit/pull/4751)).
- 📈 `st.vega_lite_chart`将对更新作出响应，特别是对输入小部件的响应。([#4736](https://github.com/streamlit/streamlit/pull/4736)).
- 💬 `st.markdown`中的长文本将始终换行。([#4696](https://github.com/streamlit/streamlit/pull/4696)).
- 📦 支持 [PDM](https://pdm.fming.dev/)（[#4724](https://github.com/streamlit/streamlit/pull/4724)）。
- ✍️ 类型：在Streamlit中更新了类型注释！（[#4679](https://github.com/streamlit/streamlit/pull/4679), [#4680](https://github.com/streamlit/streamlit/pull/4680), [#4681](https://github.com/streamlit/streamlit/pull/4681), [#4682](https://github.com/streamlit/streamlit/pull/4682), [#4683](https://github.com/streamlit/streamlit/pull/4683), [#4684](https://github.com/streamlit/streamlit/pull/4684), [#4685](https://github.com/streamlit/streamlit/pull/4685), [#4686](https://github.com/streamlit/streamlit/pull/4686), [#4687](https://github.com/streamlit/streamlit/pull/4687), [#4688](https://github.com/streamlit/streamlit/pull/4688), [#4690](https://github.com/streamlit/streamlit/pull/4690), [#4703](https://github.com/streamlit/streamlit/pull/4703), [#4704](https://github.com/streamlit/streamlit/pull/4704), [#4705](https://github.com/streamlit/streamlit/pull/4705), [#4706](https://github.com/streamlit/streamlit/pull/4706), [#4707](https://github.com/streamlit/streamlit/pull/4707), [#4708](https://github.com/streamlit/streamlit/pull/4708), [#4710](https://github.com/streamlit/streamlit/pull/4710), [#4723](https://github.com/streamlit/streamlit/pull/4723), [#4733](https://github.com/streamlit/streamlit/pull/4733))）。

## **版本 1.9.0**

_发布日期：2022 年 5 月 4 日_

**重要变更**

- 🪗 `st.json` 现在支持一个仅限关键字的参数 `expanded`，用于指定 JSON 是否默认展开（默认为 `True`）。
- 🏃‍♀️ 通过减少每次脚本运行中的冗余工作，进一步提升性能。

**其他变更**

- 🏇 当设置/取消 `disabled` 属性时，小部件将保持其值（[#4527](https://github.com/streamlit/streamlit/pull/4527)）。
- 🧪 实验性功能，使用配置`runner.fastReruns`来提高重新运行的速度。有关启用此功能时已知的问题，请参见[#4628](https://github.com/streamlit/streamlit/pull/4628)。
- 🗺️ DataFrame时间戳支持UTC偏移（除了时区表示法）（[#4669](https://github.com/streamlit/streamlit/pull/4669)）。

## **版本1.8.0**

_发布日期：2022年3月24日_

**主要变更**

- 🏃‍♀️ 数据框架应该看到性能的提升（[#4463](https://github.com/streamlit/streamlit/pull/4463))。

**其他变更**

- 🕰 `st.slider` 通过在后端删除时区转换来更好地处理时区（[#4348](https://github.com/streamlit/streamlit/pull/4358))。
- 👩‍🎨 对我们的标题进行设计改进（[#4496](https://github.com/streamlit/streamlit/pull/4496))。

## **版本1.7.0**

发布日期：2022年3月3日

**亮点**

- 介绍 `st.snow`，庆祝我们被 Snowflake 收购！在[我们的博客文章](https://blog.streamlit.io/snowflake-to-acquire-streamlit/)中获取更多信息。

## **版本 1.6.0**

_发布日期：2022年2月24日_

**其他变更**

- 🗜 WebSocket 压缩现在默认情况下已禁用，这将提高大数据帧的 CPU 和延迟性能。如果您发现增加的网络流量更具影响力，可以使用 `server.enableWebsocketCompression` 配置选项重新启用它。
- ☑️ 🔘 单选按钮和复选框在键盘导航时改善了焦点（[#4308](https://github.com/streamlit/streamlit/pull/4308)）。

## **版本 1.5.0**

发布日期：2022年1月27日

**重要更改**

- 🌟 Favicon的默认值为PNG，以支持透明度（[#4272](https://github.com/streamlit/streamlit/pull/4272)）。
- 🚦 Select Slider小部件现在具有`disabled`参数，可以删除交互性（完成所有小部件）（[#4314](https://github.com/streamlit/streamlit/pull/4314)）。

**其他变更**

- 🔤 改进了我们的markdown库，以提供更好的对HTML的支持（特别是嵌套的HTML）（[#4221](https://github.com/streamlit/streamlit/pull/4221)）。
- 📖 当存在多个展开器时，扩展器能够更好地维护其展开状态（[#4290](https://github.com/streamlit/streamlit/pull/4290)）。
- 🗳 改进了文件上传器和摄像头输入，仅在必要时调用其`on_change`处理程序（[#4270](https://github.com/streamlit/streamlit/pull/4270)）。

## **版本1.4.0**

发布日期：2022年1月13日

**亮点**

- 📸 引入`st.camera_input`，可以直接从相机上传图像。

**重要变更**

- 🚦 现在小部件有一个`disabled`参数，可以取消交互性。
- 🚮 通过在缓存函数上使用`clear()`方法，可以以编程方式清除`st.experimental_memo`和`st.experimental_singleton`。
- 📨 开发人员现在可以配置消息的最大大小，以适应Streamlit应用程序中的较大消息。请参见`server.maxMessageSize`。
- 🐍 我们正式支持Python 3.10。

**其他变更**

- 😵‍💫 在 `threading.current_thread()` 上调用 `str` 或 `repr` 不会引发 RecursionError ([#4172](https://github.com/streamlit/streamlit/issues/4172))。
- 📹 当用户取消录制权限时，优雅地停止屏幕录制 ([#4180](https://github.com/streamlit/streamlit/pull/4180))。
- 🌇 通过使用更高质量的图像双线性重采样算法来更好地缩放图像 ([#4159](https://github.com/streamlit/streamlit/pull/4159))。

## 版本 1.3.0

_发布日期：2021年12月16日_

**重要更改**

- 💯 `st.metric` 函数支持 NumPy 值。
- 🌐 在 PyDeck 中增加了对网格图层的支持。
- 📊 更新了 Plotly 图表版本，以支持最新的功能。
- 🏀 `st.spinner` 元素具有视觉动画效果。
- 🍰 `st.caption` 函数支持使用 `unsafe_allow_html` 参数在文本中使用 HTML。

**其他更改**

- 🪲 修复了一个错误：允许使用 `st.session_state` 设置 `number_input` 的值，而不会出现警告（[#4047](https://github.com/streamlit/streamlit/pull/4047)）。
- 🪲 修复了宽模式下页脚对齐的问题 ([#4035](https://github.com/streamlit/streamlit/pull/4035)).
- 🐞 修复了容器（列、展开器等）中对Graphviz和Bokeh图表的更好支持 ([#4039](https://github.com/streamlit/streamlit/pull/4039)).
- 🐞 修复了对Vega-Lite中内联数据值的支持 ([#4070](https://github.com/streamlit/streamlit/pull/4070)).
- ✍️ 类型: 更新了实验性memo和singleton装饰器的类型注解。
- ✍️ 类型：为`st.selectbox`、`st.select_slider`、`st.radio`、`st.number_input`和`st.multiselect`提供了改进的类型注释。

## 版本 1.2.0

_发布日期：2021年11月11日_

**重要变更**

- ✏️ `st.text_input`和`st.text_area`现在具有`placeholder`参数，在字段为空时显示文本。
- 📏 查看者现在可以调整`st.text_area`中的输入框大小。
- 📁 当子目录中的文件发生更改时，Streamlit可以自动重新加载。
- 🌈 我们已经升级了对Bokeh 2.4.1的支持！我们建议您将Bokeh库更新到2.4.1以保持功能正常。从现在开始，如果您的Bokeh版本不匹配，我们将通过错误提示通知您。
- 🔒 开发人员可以通过属性表示法（例如 `st.secrets.key` 而不是 `st.secrets["key"]`）访问密钥，就像会话状态一样。
- ✍️ 根据[PEP 561](https://mypy.readthedocs.io/en/stable/installed_packages.html)发布类型注释。现在，当运行mypy时，用户可以获得Streamlit的类型注释（[#4025](https://github.com/streamlit/streamlit/pull/4025)）。

**其他更改**

- 👀 视觉修复 ([#3863](https://github.com/streamlit/streamlit/pull/3863), [#3995](https://github.com/streamlit/streamlit/pull/3995), [#3926](https://github.com/streamlit/streamlit/pull/3926), [#3975](https://github.com/streamlit/streamlit/pull/3975)).
- 🍔 修复汉堡菜单 ([#3968](https://github.com/streamlit/streamlit/pull/3968)).
- 🖨️ 支持打印会话状态 ([#3970](https://github.com/streamlit/streamlit/pull/3970))。

## 版本 1.1.0

发布日期：2021年10月21日

**亮点**

- 🧠 内存改进：Streamlit 应用现在在运行过程中分配的内存大大减少。

**重要更改**

- ♻️ 当 `secrets.toml` 文件的内容发生更改时，应用程序现在会自动重新运行（在此之前，您需要手动刷新页面）。

**其他更改**

- 🔗 将一些链接重定向到我们的[全新文档网站](https://docs.streamlit.io/)，例如在异常信息中的链接。
- 🪲 修复了使用会话状态初始化范围滑块的问题 ([#3586](https://github.com/streamlit/streamlit/issues/3586))。
- 🐞 修复了使用`datetime`索引的`add_rows`时刷新图表的问题 ([#3653](https://github.com/streamlit/streamlit/issues/3653))。
- ✍️ 在代码库中添加了更多的类型注释 ([#3908](https://github.com/streamlit/streamlit/issues/3908))。

## 版本 1.0.0

_发布日期：2021年10月5日_

**亮点**

- 🎈发布 Streamlit 1.0！想了解更多，请查看我们的[1.0博客文章](https://blog.streamlit.io/announcing-streamlit-1-0/)。

**其他变更**

- 🐞 修复了在使用Arrow时，使用`df.dtypes`显示数据类型时失败的问题（[#3709](https://github.com/streamlit/streamlit/issues/3709)），图像标题保持在图像宽度内并可读取（[#3530](https://github.com/streamlit/streamlit/issues/3530)）。

## 版本 0.89.0

_发布日期：2021年9月22日_

**亮点**

- 💰 引入`st.experimental_memo`和`experimental_singleton`，这是一种新的用于缓存的原始功能！请查看[我们的博客文章](https://blog.streamlit.io/new-experimental-primitives-for-caching/)。
- 🍔 Streamlit允许开发人员配置其汉堡菜单以更加用户为中心。

**重要改变**

- 💅 我们更新了我们的UI，使用了新的字体，使其更加精美。
- 🎨 现在，当将`theme`对象发送给自定义组件时，我们支持`theme.base`。
- 🧠我们修改了会话状态，以便在任何参数发生更改时重置小部件，即使它们提供了一个键。
  - 某些小部件的行为可能已经发生了变化，但我们认为这个修改是最合理的。我们在[我们的文档](/library/advanced-features/widget-semantics)中添加了一节来描述它们的行为。

**其他变更**

- 🐞 Bug修复：支持从URL下载SVG文件（[#3809](https://github.com/streamlit/streamlit/pull/3809)）以及不以`<svg>`标签开头的SVG文件（[#3789](https://github.com/streamlit/streamlit/pull/3789)）。

## 版本 0.88.0

_发布日期：2021年9月2日_

**亮点**

- ⬇️ 引入`st.download_button`，一个新的按钮小部件，用于轻松下载文件。

**重要变更**

- 🛑 我们对Streamlit社区云的屏蔽异常体验进行了改进。当`client.showErrorDetails=true`时，异常会显示错误类型和Traceback，但会屏蔽实际的错误文本，以防止数据泄露。

## 版本 0.87.0

发布日期：2021年8月19日

**亮点**

- 🔢 引入了`st.metric`，用于显示关键绩效指标的API。查看[演示应用程序](https://streamlit-release-demos-0-87streamlit-app-0-87-rfzphf.streamlit.app/)，展示其功能。

**其他变更**

- 🐞 **错误修复**：文件上传者在关闭展开器时保留状态（[#3557](https://github.com/streamlit/streamlit/issues/3557)），`st.empty`中的setIn错误（[#3659](https://github.com/streamlit/streamlit/issues/3659)），文档中缺少IFrame嵌入（[#3706](https://github.com/streamlit/streamlit/issues/3706)），修复写入某些PNG文件时的错误（[#3597](https://github.com/streamlit/streamlit/issues/3597)）。

## 版本 0.86.0

发布日期：2021年8月5日

**亮点**

- 🎓 我们的布局基元已经从 beta 版本中毕业了！您现在可以在没有 `beta_` 前缀的情况下使用 `st.columns`、`st.container` 和 `st.expander`。

**重要变更**

- 📱 当使用 `st.columns` 时，当视口大小 `<640px` 时，列会垂直堆叠，以便在较小的视口上保持一致和更清晰的列布局。([#3594](https://github.com/streamlit/streamlit/issues/3594))。

**其他变更**

- 🐞 **Bug修复**：修复了`st.date_input`在为空的情况下崩溃的问题（[#3194](https://github.com/streamlit/streamlit/issues/3194)），修复了使用utf-8打开文件的问题（[#3022](https://github.com/streamlit/streamlit/issues/3022)），修复了`st.select_slider`在交互时重置其状态的问题（[#3600](https://github.com/streamlit/streamlit/issues/3600)）。

## 版本0.85.0

_发布日期：2021年7月22日_

**亮点**

- 🏹 Streamlit现在在将数据帧从Streamlit服务器发送到前端时使用[Apache Arrow](https://arrow.apache.org)进行序列化。请参阅我们的[博客文章](https://blog.streamlit.io/)。
  - （希望继续使用传统的数据帧序列化的用户可以在他们的`config.toml`中设置`dataFrameSerialization`配置选项为`"legacy"`）。

**其他更改**

- 🐞 修复：无响应的pydeck示例（[#3395](https://github.com/streamlit/streamlit/issues/3395)），JSON解析错误消息（[#2324](https://github.com/streamlit/streamlit/issues/2324)），工具提示渲染（[#3300](https://github.com/streamlit/streamlit/issues/3300)），在Streamlit Sharing上颜色选择器无法使用（[#2689](https://github.com/streamlit/streamlit/issues/2689)）。

## 版本 0.84.0

_发布日期：2021年7月1日_

**亮点**

- 🧠 引入`st.session_state`和小部件回调，使您能够为应用程序添加状态。请查看[博客文章](http://blog.streamlit.io/session-state-for-streamlit/)

**重要更改**

- 🪄 `st.text_input`现在有一个`autocomplete`参数，允许使用密码管理器

**其他更改**

- 使用st.set_page_config来指定页面标题不再在标题后附加“Streamlit”（[#3467](https://github.com/streamlit/streamlit/pull/3467))
- 当小部件已达到其最大值（或最小值）时，NumberInput现在会禁用加号/减号按钮（[#3493](https://github.com/streamlit/streamlit/pull/3493)）

## 版本0.83.0

发布日期：2021年6月17日

**亮点**

- 🛣️ 更新Streamlit文档，包括逐步指南，演示如何将Streamlit应用程序连接到各种数据库和API

**重要更改**

- 📄 `st.form` 现在有一个 `clear_on_submit` 参数，在提交表单时“重置”表单的所有小部件。

**其他变更**

- 修复了有关文件编码的错误（[#3320](https://github.com/streamlit/streamlit/issues/3220), [#3108](https://github.com/streamlit/streamlit/issues/3108), [#2731](https://github.com/streamlit/streamlit/issues/2731))

## 版本 0.82.0

发布日期：2021 年 5 月 13 日

**重要变更**

- ♻️ 通过在脚本运行之间强制进行垃圾回收，改进了内存管理。

## 版本 0.81.1

发布日期：2021 年 4 月 29 日

**亮点**

- 📝 引入`st.form`和`st.form_submit_button`，允许您批量输入小部件。查看我们的[博客文章](http://blog.streamlit.io/introducing-submit-button-and-forms)
- 🔤 引入`st.caption`，让您可以在应用程序的任何位置添加说明文本。
- 🎨 主题更新，包括能够构建从任何默认主题继承的主题。
- 🚀 改进了从应用菜单到Streamlit共享的部署体验。

**其他更改**

- 支持在自定义组件中处理二进制文件（[#3144](https://github.com/streamlit/streamlit/pull/3144)）
## 版本 0.80.0
_发布日期：2021年4月8日_
**亮点**
- 🔐 Streamlit 现在支持在部署到 Streamlit Sharing 的应用程序中管理 Secrets！
- ⚓️ 标题和标题现在带有自动生成的锚链接。只需将鼠标悬停在任何标题上，然后单击 🔗 获取链接！
**其他变更**

- 为自定义组件添加了`allow-downloads`功能（[#3040](https://github.com/streamlit/streamlit/issues/3040)）
- 修复了深色主题下的Markdown表格问题（[#3020](https://github.com/streamlit/streamlit/issues/3020)）
- 改进了自定义主题对话框中的颜色选择器小部件（[#2970](https://github.com/streamlit/streamlit/issues/2970)）

## 版本0.79.0

发布日期：2021年3月18日

**亮点**

- 🌈 引入了自定义主题的支持。请查看我们的[博客文章](http://blog.streamlit.io/introducing-theming/)。
- 🌚 此版本还引入了深色模式！
- 🛠️ 所有输入小部件现在支持工具提示。

**其他改动**

- 修复了有关文件编码（[#1936](https://github.com/streamlit/streamlit/issues/1936), [#2606](https://github.com/streamlit/streamlit/issues/2606)）和缓存函数（[#2728](https://github.com/streamlit/streamlit/issues/2728)）的错误。

## 版本 0.78.0

_发布日期：2021年3月4日_

**功能**

- 如果您正在使用 Streamlit for Teams beta 版本，我们对密钥的处理方式进行了一些更新。请查看 beta 版本文档获取更多信息！
- Dataframes 现在会显示所有 DateTime 和 Time 列的时区，并显示应用了时区后的时间，而不是以 UTC 时间显示。

**重要的错误修复**

- 在 `st.beta_columns` 中对列对齐进行了各种改进。
- 从 `st.image` 中删除了长期不推荐使用的 `format` 参数，并替换为 `output_format`。

## 版本 0.77.0

发布日期：2021年2月23日

**新增功能**

- 添加了一个新的配置选项 `client.showErrorDetails`，允许开发者控制错误消息的细节级别。这对于部署应用程序时，想要隐藏在回溯中包含的潜在敏感信息对用户来说是非常有用的。

**重要的 bug 修复**

- 修复了 [bug](https://github.com/streamlit/streamlit/issues/1957)，其中 `st.image` 没有正确渲染某些类型的 SVG 文件。
- 修复了一个[回归问题](https://github.com/streamlit/streamlit/issues/2699)，即仅在悬停时显示`st.slider`的当前值。

## 版本 0.76.0

_发布日期：2021年2月4日_

**重要更改**

- 🎨 [`st.color_picker`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.color_picker) 现在已经不再是 beta 版本。这意味着过去3个月被标记为弃用的旧的 beta_color_picker 函数现在已被 color_picker 替代。
- 🐍当以`python script.py`形式直接运行Streamlit脚本时，会显示警告。
- [`st.image`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.image)的`use_column_width`现在默认为`auto`选项，如果图像超过列宽，它将调整图像大小以适应列宽。
- ✂️ 修复了 [`st.beta_expander`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.beta_expander) 中内容被截断的问题 ([2437](https://github.com/streamlit/streamlit/issues/2437) 和 [2247](https://github.com/streamlit/streamlit/issues/2247))
- 📜 修复了 [`st.dataframe`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.dataframe) 中滚动条与最后一列内容重叠的 [bug](https://github.com/streamlit/streamlit/issues/2543)。
- 💾 修复了[`st.file_uploader`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.file_uploader)的[bug](https://github.com/streamlit/streamlit/issues/2561)，返回的文件数据不是最新上传的文件。
- ➕ 修复了一些LaTeX命令无法正确渲染的bug（[2086](https://github.com/streamlit/streamlit/issues/2086)和[2556](https://github.com/streamlit/streamlit/issues/2556)）。

## 版本0.75.0

发布日期：2021年1月21日

**重要变更**

- 🕳 [`st.empty`](https://docs.streamlit.io/en/0.75.0/api.html#streamlit.empty)之前在脚本结束时会清除组件。现在已更新为立即清除组件。
- 🛹 之前在宽模式下，网页周围有较窄的边距。现在已增加以提供更好的视觉体验。

## 版本 0.74.0

_发布日期：2021年1月6日_

**重要更改**

- 💾 [`st.file_uploader`](https://docs.streamlit.io/en/0.74.0/api.html#streamlit.file_uploader) 已稳定，并移除了废弃警告以及相关的配置选项 (`deprecation.showfileUploaderEncoding`)。
- 📊 在页面加载时，不再重复显示 [`st.bokeh_chart`](https://docs.streamlit.io/en/0.74.0/api.html#streamlit.bokeh_chart)。
- 🎈 修复了页面图标，以支持带有变体的表情符号（例如🤦‍♀️与🤦🏼‍♀️）或破折号（例如🌙 - 月牙）。

## 版本 0.73.0

_发布日期：2020年12月17日_

**重要变更**

- 🐍 Streamlit 现在可以在 Python 3.9 上安装。但是，Streamlit 组件目前还不兼容 Python 3.9，必须使用版本 3.8 或更早的版本。
- 🧱 Streamlit 组件现在支持同源策略，可以使用浏览器提供的功能，比如摄像头组件。
- 🐙 修复了在 Git 版本 2.7.0 或更早版本上运行时 Streamlit 分享部署体验的问题。
- 🧰 处理[`st.file_uploader`](https://docs.streamlit.io/en/0.72.0/api.html#streamlit.file_uploader)上传文件意外关闭的情况。

## 版本0.72.0

_发布日期：2020年12月2日_

**重要更改**

- 🌈 建立主题框架并迁移现有组件。
- 📱 改进移动设备上的侧边栏体验。
- 🧰 更新[`st.file_uploader`](https://docs.streamlit.io/en/0.71.0/api.html#streamlit.file_uploader)以减少重新运行。

## 版本0.71.0

_发布日期：2020年11月11日_

**重要变更**

- 📁 更新[`st.file_uploader`](https://docs.streamlit.io/en/0.71.0/api.html#streamlit.file_uploader)以在应用程序重新运行时自动重置缓冲区。
- 📊 优化图表的默认渲染方式，并减少初始渲染的问题。

## 版本0.70.0

_发布日期：2020年10月28日_

**重要变更**

- 🧪 [`st.set_page_config`](https://docs.streamlit.io/zh_CN/0.70.0/api.html#streamlit.set_page_config) 和 [`st.color_picker`](https://docs.streamlit.io/zh_CN/0.70.0/api.html#streamlit.color_picker) 已经移动到 Streamlit 命名空间中。它们将在2021年1月28日之前的beta版本中被移除。了解更多关于我们的beta过程，请点击[此处](https://docs.streamlit.io/zh_CN/0.70.0/api.html#beta-and-experimental-features)。
- 📊 改进离散值的柱状图显示。

## 版本 0.69.0

_发布日期：2020年10月15日_

**亮点：**

- 🎁 引入了Streamlit共享功能，这是部署、管理和共享您的公共Streamlit应用程序的最佳方式，而且是免费的。在我们的[博客文章](http://blog.streamlit.io/introducing-streamlit-sharing/)上了解更多信息，或在[这里](https://streamlit.io/sharing)注册！
- 添加了`st.experimental_rerun`以编程方式重新运行您的应用程序。感谢[SimonBiggs](https://github.com/SimonBiggs)！

**重要变更**

- 📹 对于st.video，增强了在各种浏览器上的开始和停止时间的支持。
- 🖼 修复了媒体文件偶尔失败的问题。
- 📦 修复了自定义组件与Safari的兼容性问题。请确保升级到最新的[streamlit-component-lib](https://www.npmjs.com/package/streamlit-component-lib)。

## 版本 0.68.0

发布日期：2020年10月8日

**亮点：**

- ⌗ 推出了Streamlit的新布局选项！垂直布局，让位给新布局。
  为水平布局留出一些空间！请查看我们的[博客文章](https://blog.streamlit.io/introducing-new-layout-options-for-streamlit/)。
- 💾 文件上传器重新设计，具有新功能以支持多文件上传和更好地处理已上传的文件。这可能导致一些破坏性更改。请参阅我们的[文档](https://docs.streamlit.io/en/0.68.0/api.html#streamlit.file_uploader)中的新API。

**重要更改**

- 🎈 `st.balloon` 通过更漂亮的气球和更平滑的动画进行了改进。
- 🚨 破坏性变更：在2020年1月之后，我们已完全移除了 `st.deck_gl_chart` 的 API。请改用 `st.pydeck_chart`。
- 🚨 破坏性变更：在2020年1月之后，对于 `st.altair_chart`、`st.graphviz_chart`、`st.plotly_chart` 和 `st.vega_lite_chart` 的 `width` 和 `height`，我们已完全移除了这些参数。
  请在相应的图表库中设置宽度和高度。

## 版本0.67.0

发布日期：2020年9月16日

**亮点：**

- 🦷 Streamlit组件现在可以返回字节给您的Streamlit应用程序。要创建一个返回字节的组件，请确保升级到最新的[streamlit-component-lib](https://www.npmjs.com/package/streamlit-component-lib)。

**重要更改：**

- 📈 弃用警告：从2020年12月1日开始，`st.pyplot()`将需要一个图表作为参数。
  提供了以下内容。要禁用弃用警告，请将`deprecation.showPyplotGlobalUse`设置为`False`。
- 🎚 当处理大型数据集时，`st.multiselect`和`st.select`现在运行速度非常快。感谢 [masa3141](https://github.com/masa3141)！

## 版本0.66.0

发布日期：2020年9月1日

**亮点：**

- ✏️ 现在可以在侧边栏中使用`st.write`！
- 🎚 可以使用`st.select_slider`获取具有不同或非数值的滑块。
- ⌗ Streamlit组件现在可以将数据帧返回给您的Streamlit应用程序。请查看我们的[SelectableDataTable示例](https://github.com/streamlit/component-template/tree/master/examples/SelectableDataTable)。
- 📦 我们在Streamlit组件模板中使用的Streamlit组件库现在作为npm包（[streamlit-component-lib](https://www.npmjs.com/package/streamlit-component-lib)）可用，以简化将来升级到最新版本的操作。
  现有组件无需迁移。

**重要更改**

- 🐼 支持来自pandas 1.0.0版本的StringDtype
- 🧦 支持在Unix套接字上运行Streamlit

## 版本0.65.0

_发布日期：2020年8月12日_

**亮点：**

- ⚙️ 通过st.beta_set_page_config()函数可以设置页面标题、favicon、侧边栏状态和宽模式。详细信息请参阅我们的[文档](https://docs.streamlit.io/en/0.65.0/api.html#streamlit.set_page_config)。
- 📝 通过使用st.experimental_set_query_params和st.experimental_get_query_params来添加具有状态的行为，并通过查询参数进行控制。感谢[@zhaoooyue](https://github.com/zhaoooyue)！
- 🐼 改进了对st.radio，st.selectbox和st.multiselect的pandas dataframe支持。
- 🛑 使用st.stop可以退出Streamlit应用程序的执行。
- 🖼 支持在st.image中使用内联SVG。

**提示：**

- 🚨 弃用警告：st.image的参数格式已更名为output_format。

## 版本0.64.0

_发布日期：2020年7月23日_

**亮点：**

- 📊 默认使用紧凑布局显示matplotlib图表。要禁用此功能，请将`bbox_inches`设置为`None`、字符串形式的英寸或`Bbox`对象。
- 🗃 在`st.file_uploader`上自动编码的功能已弃用，并会发出警告。
- 🙈 如果`gatherUserStats`设置为`False`，则不会加载Segment库。感谢[@tanmaylaud](https://github.com/tanmaylaud)的贡献！

## 版本0.63.0

_发布日期：2020年7月13日_

**亮点：**

- 🧩 **支持 Streamlit 组件！！！** 详细信息请参阅[文档](https://docs.streamlit.io/en/latest/streamlit_components.html)。
- 🕗 在 [`st.slider`](https://docs.streamlit.io/en/latest/api.html#streamlit.slider) 中支持日期时间。当然，就像在 `st.slider` 中使用的任何其他值一样，您也可以传入两个元素的列表来获取一个日期时间范围滑块。

## 版本 0.62.0

_发布日期：2020 年 6 月 21 日_

**亮点：**

- 📨 通过配置选项 `server.enableWebsocketCompression`，可以开启/关闭WebSocket压缩功能。如果您的服务器剥离HTTP头并且无法更改该行为，这将非常有用。
- 🗝️ 使用[Cookie-to-header token](https://en.wikipedia.org/wiki/Cross-site_request_forgery#Cookie-to-header_token)技术，Streamlit提供了开箱即用的CSRF保护支持。这意味着如果您从多个副本提供Streamlit应用程序，则需要
  将它们配置为使用相同的cookie密钥，使用`server.cookieSecret`配置选项。
要关闭XSRF保护，请将`server.enableXsrfProtection`设置为false。

**显著的错误修复:**

- 🖼️ 添加了一个优雅期来修复与图像缓存过期逻辑相关的多个错误，
其中使用`st.image`或`st.pyplot`发送的图像有时会丢失。

## 版本0.61.0

_发布日期: 2020年6月2日_

**亮点:**

- 📅 支持在`st.date_picker`中使用日期范围。请参阅
  [docs](https://docs.streamlit.io/en/latest/api.html#streamlit.date_picker)
  了解更多信息，但简而言之就是：只需将列表/元组作为默认日期传递，它将被解释为一个范围。
- 🗣️ 现在，您可以选择`st.echo`是在输出块上方还是下方打印代码。要了解更多信息，请参阅[docs](https://docs.streamlit.io/en/latest/api.html#streamlit.echo)中的`code_location`参数。
- 📦 改进了对Keras模型和Tensorflow `saved_models`的`@st.cache`支持。

## 版本0.60.0

_发布日期：2020年5月18日_

**亮点：**

- ↕️ 可以使用`height`参数设置`st.text_area`的高度（以像素表示）。详见[文档](https://docs.streamlit.io/en/latest/api.html#streamlit.text_area)。
- 🔡 可以设置`st.text_area`或`st.text_input`中允许的最大字符数。请查看`max_chars`参数。
  [文档](https://docs.streamlit.io/en/latest/api.html#streamlit.text_area)。
- 🗺️ 对[H3](https://h3geo.org/)地理空间索引系统的DeckGL支持更好。现在您可以在[`st.pydeck_chart`](https://docs.streamlit.io/en/latest/api.html#streamlit.pydeck_chart)中使用`H3HexagonLayer`等功能。
- 📦 改进了对PyTorch TensorBase和Model的`@st.cache`支持。

## 版本0.59.0

发布日期：2020年5月5日

**亮点：**

- 🎨 新的颜色选择器小部件！与其一起使用
  [`st.beta_color_picker()`](https://docs.streamlit.io/en/0.69.0/api.html#streamlit.beta_color_picker)
- 🧪 引入 `st.beta_*` 和 `st.experimental_*` 函数前缀，以加快Streamlit功能的发布速度。更多信息请参见[文档](https://docs.streamlit.io/en/latest/api.html#pre-release-features)。
- 📦 改进了对SQL Alchemy对象、CompiledFFI、PyTorch张量和`builtins.mappingproxy`的`@st.cache`支持。

## 版本0.58.0

_发布日期：2020年4月22日_

**亮点：**

- 💼 使 `st.selectbox` 的筛选不区分大小写。
- ㈬ 更好地支持 `@st.cache` 中的Tensorflow会话。
- 📊 改变了 `st.pyplot` 的行为，只在使用全局Matplotlib图形时自动清除图形（即仅在调用 `st.pyplot()` 而不是 `st.pyplot(fig)` 时）。

## 版本 0.57.0

发布日期：2020年3月26日

**亮点：**

- ⏲️ 通过设置 `max_entries` 和 `ttl` 参数，为 `@st.cache` 中的函数设置过期选项。请参阅
  [文档](https://docs.streamlit.io/en/latest/api.html#streamlit.cache)。
- 🆙 改进了`st.file_uploader`的机制，现在性能更好了！还将默认上传限制增加到200MB（可通过`server.max_upload_size`进行配置）。
- 🔒 `server.address`配置选项现在将服务器绑定到该地址以提高安全性。
- 📄 为`@st.cache`的错误消息添加了更多细节，以便更容易进行调试。

## 版本0.56.0

_发布日期：2020年2月15日_

**亮点：**

- 📄 对 st.cache 的错误消息进行了改进。错误现在还指向我们刚发布的新缓存文档。点击[这里](https://discuss.streamlit.io/t/help-us-stress-test-streamlit-s-latest-caching-update/1944)了解更多！

**重大变更：**

- 🐍 如[上个月宣布的](https://discuss.streamlit.io/t/streamlit-will-deprecate-python-2-in-february/1656)，**Streamlit 不再支持 Python 2**。要使用 Streamlit，您需要 Python 3.5 或更高版本。

## 版本 0.55.0

_发布日期：2020年2月4日_

**亮点：**

- 📺 **能够直接从Streamlit录制屏幕录像！** 这使您可以轻松记录和分享有关您的模型、分析、数据等的解释。只需点击 ☰ 然后选择“录制屏幕录像”。试试看吧！

## 版本0.54.0

_发布日期：2020年1月29日_

**亮点：**

- ⌨️ 支持密码字段！只需将 `type="password"` 传递给 `st.text_input()`。

**值得注意的修复：**

- ✳️ 多项st.cache的改进，包括对复杂对象的更好支持。
- 🗣️ 修复了侧边栏在多个用户之间的交叉对话。

**重大变更:**

- 如果您正在使用SessionState <del>hack</del> Gist，您应该重新下载它！
  根据您使用的是哪种hack，以下是一些链接可以节省您的时间:
  - [SessionState.py](https://gist.github.com/tvst/036da038ab3e999a64497f42de966a92)
  - [st_state_patch.py](https://gist.github.com/tvst/0899a5cdc9f0467f7622750896e6bd7f)

## 版本 0.53.0

发布日期：2020年1月14日

**亮点：**

- 🗺️ 支持所有 DeckGL 功能！只需要使用[Pydeck](https://deckgl.readthedocs.io/en/latest/)替代[`st.deck_gl_chart`](https://docs.streamlit.io/en/latest/api.html#streamlit.pydeck_chart)。只需将 PyDeck 对象传递给[`st.pydeck_chart`](https://docs.streamlit.io/en/latest/api.html#streamlit.pydeck_chart)即可。
  [`st.write`](https://docs.streamlit.io/en/latest/api.html#streamlit.write)或[magic](https://docs.streamlit.io/en/latest/api.html#magic)。

请注意，由于这是一个**预览版**，在不久的将来可能会有所变化。在我们稳定API之前，期待听取社区的意见！

**目标是取代`st.deck_gl_chart`**，因为它可以做到旧API所做的所有事情，**还有更多！**

- 🆕 在开发过程中更好地处理Streamlit的升级。现在，如果应用程序使用的是比浏览器选项卡运行的Streamlit版本更新的版本，我们会自动重新加载浏览器选项卡。

- 👑 新的网站图标，带有我们的新标志！

**修复的问题:**

- 在Python 3.8中，魔术命令现在可以正常工作了。它不再导致文档字符串在应用程序中呈现。

**破坏性变更:**

- 更新了我们计算所有图表类型的默认宽度和高度的方式。
  现在，我们将图表大小的设置交给图表库自身处理，请参考库的文档。

因此，大多数图表命令已经不再使用`width`和`height`参数，而是引入了`use_container_width`参数，它允许您使图表占据尽可能多的水平空间（这曾经是默认设置）。

## 版本 0.52.0

_发布日期：2019年12月20日_

**亮点：**

- 📤 文件上传小部件的预览版本。只需调用 [`st.file_uploader`](https://docs.streamlit.io/en/latest/api.html#streamlit.file_uploader) 就可以尝试它！

  _请注意，作为**预览版**，在不久的将来可能会有所更改。在我们稳定API之前，期待从社区听到反馈！_

- 👋 在 `st.write` 和 `st.markdown` 中支持[表情代码](https://www.webfx.com/tools/emoji-cheat-sheet/)！尝试使用 `st.write("Hello :wave:")`。

**重大变更:**

- 🧹 `st.pyplot` 现在默认清除图表，因为这是99%情况下的预期行为。这样您就可以创建两个或更多的Matplotlib图表，而无需每次都调用[`pyplot.clf`](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.clf.html)。如果您想关闭此行为，请使用[`st.pyplot(clear_figure=False)`](https://docs.streamlit.io/en/latest/api.html#streamlit.pyplot)。
- 📣 `st.cache`不再检查输入的变化。这是我们持续努力简化缓存系统并为Streamlit的其他缓存原语（如会话状态）的发布做准备的第一个更改！

## 版本0.51.0

_发布日期：2019年11月30日_

**亮点：**

- 🐕 现在您可以使用配置选项`server.fileWatcherType`来调整文件监视器的行为。可以在以下选项之间切换：
  - `auto`（默认）：Streamlit将尝试使用watchdog模块，
    如果 watchdog 不可用，则回退到轮询模式。
  - `watchdog`：强制 Streamlit 使用 watchdog 模块。
  - `poll`：强制 Streamlit 始终使用轮询。
  - `none`：Streamlit 不会监视文件。

**显著的错误修复：**

- 修复静态报告共享中的 "keyPrefix" 选项 [#724](https://github.com/streamlit/streamlit/pull/724)
- 为 DeckGL 图表添加 getColorX 和 getTargetColorX 支持 [#718](https://github.com/streamlit/streamlit/pull/718)
- 修复了在Windows上使用Python 3.8时的Tornado问题 [#682](https://github.com/streamlit/streamlit/pull/682)
- 在Linux上如果没有安装xdg-open，则回退到webbrowser [#701](https://github.com/streamlit/streamlit/pull/701)
- 修复了Firefox中数字输入旋钮的问题 [#683](https://github.com/streamlit/streamlit/pull/683)
- 修复了Windows上的CTRL+ENTER问题 [#699](https://github.com/streamlit/streamlit/pull/699)
- 当处于无头模式时，不会自动创建凭据文件 [#467](https://github.com/streamlit/streamlit/pull/467)

## 版本 0.50.1

_发布日期：2019 年 11 月 10 日_

**亮点：**

- 👩‍🎓 支持 SymPy，并能够使用 LaTeX 绘制数学表达式！请查看
  [`st.latex`](/library/api-reference/text/st.latex)，
  [`st.markdown`](/library/api-reference/text/st.markdown)，
  和
  [`st.write`](/library/api-reference/write-magic/st.write)。
- 🌄 现在可以使用环境变量设置配置选项。例如，`export STREAMLIT_SERVER_PORT=9876`。
- 🐱 可以直接使用Github和Gist的URL调用`streamlit run`，不需要先获取"raw" URL！
- 📃 更清晰的异常堆栈跟踪。现在，我们从用户应用程序的堆栈跟踪中删除所有Streamlit特定的代码。

## 版本 0.49.0

发布日期：2019年10月23日

**亮点:**

- 💯 新的输入小部件，可使用键盘输入数字：`st.number_input()`
- 📺 音频/视频改进：支持从 URL 加载，嵌入 YouTube 视频，并设置开始位置。
- 🤝 您现在可以（再次）将静态快照分享到 S3！请查看 `streamlit config show` 中的 S3 部分进行设置。然后从右上角菜单分享。
- ⚙️ 使用 `server.baseUrlPath` 配置选项将 Streamlit 的 URL 设置为类似 `http://domain.com/customPath` 的形式。

**显著的错误修复:**

- 修复了多个Windows上的错误，包括[问题
  #339](https://github.com/streamlit/streamlit/issues/399)和
  [#401](https://github.com/streamlit/streamlit/issues/301)。

## 版本0.48.0

_发布日期：2019年10月12日_

**亮点:**

- 🔧 可以将配置选项作为命令行标志或在本地配置文件中设置。
- ↕️ 现在可以最大化图表和图片了！
- ⚡ 在快速连续写入应用数据时，Streamlit现在更快了。
- ✳️ 支持从“保存时运行”和`@st.cache`散列中屏蔽文件夹模式。
- 🎛️ 在修改Python文件时改进小部件状态的处理方式。
- 🙈 在`st.write`和`st.markdown`中改进了对HTML的支持。但是HTML仍然不安全！

**值得注意的错误修复:**

- 修复了与将Python环境设置为当前工作目录相关的`@st.cache`错误。[问题 #242](https://github.com/streamlit/streamlit/issues/242)
- 修复了在Windows上加载根URL `/`的问题。[Issue #244](https://github.com/streamlit/streamlit/issues/244)

## 版本0.47.0

_发布日期：2019年10月1日_

**亮点：**

- 🌄 新的hello.py展示了4个令人惊叹的Streamlit应用程序。快来试试吧！
- 🔄 当8501端口已被占用时，Streamlit现在会自动选择一个未使用的端口。
- 🎁 侧边栏支持现已正式发布！只需将任何命令以 `st.sidebar.` 开头，而不是 `st.`。
- ⚡ 性能改进：我们在websocket层添加了缓存，因此当数据在运行之间没有变化时，我们不再向浏览器重新发送数据
- 📈 我们的“本地”图表 `st.line_chart`、`st.area_chart` 和 `st.bar_chart` 现在在幕后使用Altair
- 🔫 改进的小部件：自定义st.slider标签；在多选中添加默认值
- 🕵️‍♀️ 文件系统监视器现在忽略隐藏的文件夹和虚拟环境
- 💅 还有很多关于缓存和小部件状态管理的改进

**重大变更：**

- 🛡️我们暂时禁用了对Streamlit应用程序的静态“快照”共享支持。现在我们已经不再处于有限访问的测试阶段，我们需要确保共享功能经过深思熟虑并遵守诸如DMCA（数字千年版权法）等法律。但我们正在寻找解决方案！

## 版本0.46.0

_发布日期：2019年9月19日_

**亮点：**

- ✨ 魔术指令！无需输入`st.write`，即可使用`st.write`。请参考
  <https://docs.streamlit.io/en/latest/api.html#magic-commands>
- 🎛️ 新增 `st.multiselect` 组件。
- 🐍 修复了许多安装问题，现在您可以使用 `pip install streamlit`，即使在 Conda 环境中也可以使用！因此，我们已停用了我们的 Conda 仓库。
- 🐞 多个错误修复和额外的改进，为我们的发布做准备！

**重大变更：**

- 🛡️ 默认情况下，`st.write`/`st.markdown` 中禁止使用 HTML 标签。更多信息和临时解决方法请参考：<https://github.com/streamlit/streamlit/issues/152>

## 版本 0.45.0

_发布日期: 2019年8月28日_

**亮点:**

- 😱 对侧边栏的实验性支持！如果您想成为beta测试人员，请告诉我们。
- 🎁 完全重设计的`st.cache`！性能更高，API更简洁，支持缓存被`@st.cached`函数调用的函数，友好的错误提示信息等等！
- 🖼️ `st.image`速度极快，可以选择JPEG和PNG压缩格式，以及RGB和BGR（用于OpenCV）。
- 💡 `st.slider`、`st.selectbox`和`st.radio`的更智能的API。
- 🤖 自动修复Matplotlib后端，无需编辑.matplotlibrc文件。

## 版本0.44.0

发布日期：2019年7月28日

**亮点：**

- ⚡ 当您在Streamlit代码中执行ctrl-c/rerun时，快速重新连接
- 📣 连接失败时提供有用的错误消息
- 💎 修复了多个错误并改进了新发布的交互式小部件的外观和功能

## 版本0.43.0

发布日期：2019年7月9日

**亮点：**

- ⚡ 支持交互式小部件！🎈🎉

## 版本0.42.0

_发布日期：2019年7月1日_

**亮点：**

- 💾 可以将Vega-Lite和Altair图表保存为SVG或PNG格式
- 🐇 现在在浏览器中缓存JS文件以加快加载速度
- ⛔ 改进了Streamlit应用程序内的错误处理

## 版本0.41.0

_发布日期：2019年6月24日_

**亮点：**

- 📈 大大改善了对Vega-Lite和Altair中命名数据集的支持
- 🙄 增加了在监视文件更改时忽略某些文件夹的能力。请参阅 `server.folderWatchBlacklist` 配置选项。
- ☔ 更加健壮，可以处理用户脚本和导入模块的语法错误

## 版本 0.40.0

_发布日期：2019 年 6 月 10 日_

**亮点：**

- Streamlit 的速度提高了 10 倍以上。只需保存并观察您的分析即时更新。
- 我们改变了运行 Streamlit 应用的方式：
  `$ streamlit run your_script.py [script args]`
- 与以前的Streamlit版本不同，`streamlit run [script] [script args]`会创建一个服务器（现在您无需担心代理是否正常工作）。要关闭服务器，您只需要按下**Ctrl+c**。

**为什么这样更快？**

现在，Streamlit会保持一个Python会话的运行状态，直到您关闭服务器。这意味着Streamlit可以在不启动新进程的情况下重新运行您的代码；导入的库会被缓存在内存中。另一个好处是，`st.cache`现在将缓存到内存而不是磁盘上。

**如果我以旧的方式运行Streamlit会发生什么？**

如果您运行`$ python your_script.py`，脚本将从上到下执行，但不会生成一个Streamlit应用程序。

**新架构的限制是什么？**

- 要切换Streamlit应用程序，首先您必须使用**Ctrl-c**终止Streamlit服务器。然后，您可以使用`streamlit run`命令生成下一个应用程序。
- Streamlit仅在Python文件中使用时有效，无法在Python REPL中进行交互式使用。

**还有什么需要我知道的吗？**

- 当**liveSave**开启时，我们在命令行中打印的字符串已经过清理。您可能需要调整依赖于这些字符串的任何正则表达式。
- 一些配置选项已更名：

  | 旧配置                      | 新配置                   |
  | -------------------------- | --------------------- |
  | proxy.isRemote             | server.headless       |
  | proxy.liveSave             | server.liveSave       |
  | proxy.runOnSave            | server.runOnSave      |
  | proxy.watchFileSystem      | server.runOnSave      |
  | proxy.enableCORS           | server.enableCORS     |
  | proxy.port                 | server.port           |
  | browser.proxyAddress       | browser.serverAddress |
  | browser.proxyPort          | browser.serverPort    |
  | client.waitForProxySecs    | _n/a_                 |
  | client.throttleSecs        | _n/a_                 |
  | client.tryToOutliveProxy   | _n/a_                 |
  | client.proxyAddress        | _n/a_                 |
  | client.proxyPort           | _n/a_                 |
  | proxy.autoCloseDelaySecs   | _n/a_                 |
  | proxy.reportExpirationSecs | _n/a_                 |

**如果出现故障怎么办？**

如果新的Streamlit不起作用，请通过Slack或电子邮件告知我们。您可以随时使用以下命令降级：

```bash
pip install --upgrade streamlit==0.37
```

```bash
conda install streamlit=0.37
```

**接下来是什么？**

感谢您一路陪伴！这个版本的Streamlit为交互式小部件打下了基础，这是我们非常期待与您分享的Streamlit的新功能，在接下来的几个月里会向您展示。

## 版本 0.36.0

_发布日期：2019年5月3日_

**亮点**

- 🚣‍♀️ `st.progress()` 现在也接受0.0-1.0之间的浮点数
- 🤯 改进了DataFrames中长标题的渲染效果
- 🔐 共享应用现在默认使用HTTPS

## 版本 0.35.0

_发布日期：2019年4月26日_

**亮点**

- 📷 支持 Bokeh! 请查看 `st.bokeh_chart` 的文档
- ⚡️ 改进了保存的应用程序的大小和加载时间
- ⚾️ 在整个代码库中实现了更好的错误捕获机制