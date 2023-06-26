---
title: 更新日志
slug: /library/changelog
---

# 更新日志

本页面列出了Streamlit官方发布版本的亮点、错误修复和已知问题。如果您想了解关于夜间版本、测试功能或实验性功能的信息，请参阅[尝试预发布功能](/library/advanced-features/prerelease)。

<Tip>

要升级到最新版本的Streamlit，请运行：

```bash
pip install --upgrade streamlit
```

</Tip>

## **版本 1.23.0**

发布日期：2023年6月1日

**亮点**

- ✂️ 宣布 [st.data_editor](/library/api-reference/data/st.data_editor) 的正式可用，该小部件允许您在表格式的用户界面中编辑 DataFrame 和许多其他数据结构。**重要更改：** 在 `st.session_state` 中使用的数据编辑器的表示方式已更改。了解有关新格式的更多信息，请参阅[访问编辑后的数据](/library/advanced-features/dataframes#access-edited-data)。
- ⚙️介绍了[列配置 API](/library/api-reference/data/st.column_config)，提供了一套方法来配置`st.dataframe`和`st.data_editor`列的显示和编辑行为（例如标题、可见性、类型或格式）。请留意接下来两周内发布的详细[博客文章](https://blog.streamlit.io/)和深入的[文档](/library/advanced-features/dataframes#configuring-columns)。
- 🔌 通过新的[连接到数据](/library/advanced-features/connecting-to-data)文档和[视频教程](https://www.youtube.com/watch?v=xQwDfW7UHMo)，学习如何使用`st.experimental_connection`在您的应用程序中创建和管理数据连接。

**重要变更**

- 📊 Streamlit 现在支持 Protobuf 4 和 Altair 5 ([#6215](https://github.com/streamlit/streamlit/issues/6215), [#6618](https://github.com/streamlit/streamlit/pull/6618), [#5626](https://github.com/streamlit/streamlit/issues/5626), [#6622](https://github.com/streamlit/streamlit/pull/6622)).
- ☎️ st.dataframe和st.data_editor可以使用`hide_index`隐藏索引列，使用`column_order`指定列的显示顺序，使用`disabled`参数禁用单个列的编辑功能。
- ⏱️ [st.cache_data](/library/api-reference/performance/st.cache_data)和[st.cache_resource](/library/api-reference/performance/st.cache_resource)中的`tll`参数接受格式化字符串，因此您可以简单地使用 `ttl="30d"`，`ttl="1h30m"` 或任何其他由[Pandas的Timedelta构造函数](https://pandas.pydata.org/docs/reference/api/pandas.Timedelta.html)支持的 `w`，`d`，`h`，`m`，`s`的组合 ([#6560](https://github.com/streamlit/streamlit/pull/6560))。
- 📂 `st.file_uploader`现在更准确地解释`type`参数。例如，"jpg"或".jpg"现在可以接受"jpg"和"jpeg"扩展名。这个功能还扩展到"mpeg/mpg"，"tiff/tif"，"html/htm"和"mpeg4/mp4"。
- 🤫 新的 `global.disableWidgetStateDuplicationWarning` 配置选项允许静默设置小部件默认值和键入会话状态值同时触发的警告（[#3605](https://github.com/streamlit/streamlit/issues/3605), [#6640](https://github.com/streamlit/streamlit/pull/6640)）。感谢 [@antonAce](https://github.com/antonAce)！

**其他变更**

- 🏃‍♀️通过延迟加载某些依赖项改进了启动时间（[#6531](https://github.com/streamlit/streamlit/pull/6531)）。
- 👋由于废弃和低使用率，移除了`st.beta_*`和`st.experimental_show`（[#6558](https://github.com/streamlit/streamlit/pull/6558)）
- 🚀对st.dataframe和st.data_editor进行了进一步改进：
  - 在移动设备上改进了数据编辑器的编辑功能（[#6548](https://github.com/streamlit/streamlit/pull/6548)）。
  - 所有可编辑的列在其列标题上都有一个图标，并支持工具提示（[#6550](https://github.com/streamlit/streamlit/pull/6550), [#6561](https://github.com/streamlit/streamlit/pull/6561)）。
  - 为包含日期时间、日期或时间值的列启用编辑功能（[#6025](https://github.com/streamlit/streamlit/pull/6025)）。
  - 数据编辑器中列的新输入验证选项，例如对于文本列的`max_chars`和`validate`，以及对于数值列的`min_value`、`max_value`和`step`（[#6563](https://github.com/streamlit/streamlit/pull/6563)）。
  - 数据编辑器中改进了类型解析能力（[#6551](https://github.com/streamlit/streamlit/pull/6551)）。
  - 在返回的数据结构中将缺失值统一为`None`（[#6544](https://github.com/streamlit/streamlit/pull/6544)）。
  - 当整数超过最大安全值`(2^53) -1`时，单元格中会显示警告信息（[#6311](https://github.com/streamlit/streamlit/issues/6311), [#6549](https://github.com/streamlit/streamlit/pull/6549)）。
  - 通过显示警告信息来防止编辑会话状态（[#6634](https://github.com/streamlit/streamlit/pull/6634)）。
  - 修复了列表列有时会破坏前端的问题（[#6644](https://github.com/streamlit/streamlit/pull/6644)）。
  - 修复了使用类别数据类型的索引列显示问题（[#6680](https://github.com/streamlit/streamlit/issues/6680), [#6598](https://github.com/streamlit/streamlit/pull/6598)）。
  - 修复了在添加空行时无法重新运行的问题（[#6598](https://github.com/streamlit/streamlit/pull/6598)）。
  - 统一了`st.data_editor`和`st.dataframe`的行为，根据输入的数据自动隐藏索引列（[#6659](https://github.com/streamlit/streamlit/issues/6659)，[#6598](https://github.com/streamlit/streamlit/pull/6598)）
- 🛡️ Streamlit的[安全策略](https://github.com/streamlit/streamlit/blob/develop/SECURITY.md)可以在其GitHub仓库中找到（[#6666](https://github.com/streamlit/streamlit/pull/6666)）。
- 🤏 对 `st.number_input` 和 `st.slider` 的整数大小限制进行了文档化 ([#6724](https://github.com/streamlit/streamlit/pull/6724)).
- 🐍 Streamlit 的大部分 Python 依赖项已经设置了最大允许版本，并将标准上限设置为下一个主要版本，但不包括该版本 ([#6691](https://github.com/streamlit/streamlit/pull/6691)).
- 💅 对应用程序模态框进行了用户界面设计改进 ([#6688](https://github.com/streamlit/streamlit/pull/6688)).
- 🐞 修复：`st.date_input`在暗黑模式下日期选择器可见度一致（[#6072](https://github.com/streamlit/streamlit/issues/6072)，[#6630](https://github.com/streamlit/streamlit/pull/6630)）。
- 🐜 修复：多页面应用程序中的侧边栏导航展开指示器已恢复（[#6731](https://github.com/streamlit/streamlit/pull/6731)）。
- 🐛 修复了`st.set_page_config`的文档字符串和异常消息，明确说明该命令可以在多页面应用程序中的每个页面上调用一次，而不是每个应用程序调用一次（[#6594](https://github.com/streamlit/streamlit/pull/6594)）。
- 🐝 错误修复：在渲染时，`st.json` 不再将键和值中的多个空格折叠为单个空格（[#6657](https://github.com/streamlit/streamlit/issues/6657), [#6663](https://github.com/streamlit/streamlit/pull/6663))。

## **版本 1.22.0**

_发布日期：2023 年 4 月 27 日_

**亮点**

- 🔌 引入 `st.experimental_connection`：使用我们的新连接功能，轻松连接您的应用程序到数据源和 API。在[API 参考](/library/api-reference/connections)中查找更多详细信息，并敬请期待即将发布的博客文章和详细文档！与此同时，探索我们更新的[MySQL](/knowledge-base/tutorials/databases/mysql)和[Snowflake](/knowledge-base/tutorials/databases/snowflake)连接教程，了解此功能的示例。

**重要变更**

- 🐼 Streamlit 现在支持 Pandas 2.0 ([#6413](https://github.com/streamlit/streamlit/issues/6413), [#6378](https://github.com/streamlit/streamlit/pull/6378), [#6507](https://github.com/streamlit/streamlit/pull/6507))。感谢 [connortann](https://github.com/connortann)！
- 🍔 使用`client.toolbarMode` [配置选项](https://docs.streamlit.io/library/advanced-features/configuration#view-all-configuration-options)（[#6174](https://github.com/streamlit/streamlit/pull/6174)）自定义工具栏、选项菜单和设置对话框中项目的可见性。
- 🪵 Streamlit日志现在位于“streamlit”命名空间中，而不是根记录器，使应用程序开发人员能够更好地管理日志处理（[#3978](https://github.com/streamlit/streamlit/issues/3978)，[#6377](https://github.com/streamlit/streamlit/pull/6377)）。

**其他变更**

- 🔏 无法再使用CLI参数设置敏感配置值（[#6376](https://github.com/streamlit/streamlit/pull/6376)）。
- 🤖 通过减少日志噪音，改进了调试体验（[#6391](https://github.com/streamlit/streamlit/pull/6391)）。
- 🐞 修复了bug：`@st.cache_data` 装饰的函数支持 UUID 对象作为参数（[#6440](https://github.com/streamlit/streamlit/issues/6440), [#6459](https://github.com/streamlit/streamlit/pull/6459)）。
- 🐛 修复了一个问题：现在在按钮和其他元素上通过Tab键切换时，只有在焦点在其上时才显示红色边框，而不是在点击时也显示红色边框（[#6373](https://github.com/streamlit/streamlit/pull/6373)）。
- 🪲 修复了一个问题：`st.multiselect`的清除图标更大，并包含了悬停效果（[#6471](https://github.com/streamlit/streamlit/pull/6471)）。
- 🐜 修复：自定义主题字体设置不再应用于代码块（[#6484](https://github.com/streamlit/streamlit/issues/6484), [#6535](https://github.com/streamlit/streamlit/pull/6535))。
- ©️ 修复：当鼠标悬停在代码块上时，`st.code`的复制到剪贴板按钮会显示出来（[#6490](https://github.com/streamlit/streamlit/issues/6490), [#6498](https://github.com/streamlit/streamlit/pull/6498))。

## **版本 1.21.0**

发布日期：2023年4月6日

**亮点**

- 📏 介绍 `st.divider` — 一个在您的应用程序中显示水平线的命令。在 [API 参考](/library/api-reference/text/st.divider) 中了解如何使用该命令。
- 🔏 Streamlit 现在支持使用全局的 `secrets.toml` 文件，除了项目级别的文件外，以便轻松存储和安全访问您的秘密。在 [秘密管理](/library/advanced-features/secrets-management) 中了解更多信息。
- 🚀 [st.help](/library/api-reference/utilities/st.help) 已经进行了改进，可以显示有关对象方法、属性、类等更多信息，非常适用于调试（[#5857](https://github.com/streamlit/streamlit/pull/5857), [#6382](https://github.com/streamlit/streamlit/pull/6382))！

**重要更改**

- 🪜 [st.time_input](/library/api-reference/widgets/st.time_input) 支持使用关键字参数 `step` 添加一个步进间隔 ([#6071](https://github.com/streamlit/streamlit/pull/6071)).
- ❓ 大多数 [文本元素](/library/api-reference/text) 可以使用 `help` 参数添加工具提示 ([#6043](https://github.com/streamlit/streamlit/pull/6043)).
- ↔️ [st.pyplot](/library/api-reference/charts/st.pyplot) 现在有一个 `use_container_width` 参数，可以将图表设置为容器的宽度（现在所有的 [图表元素](/library/api-reference/charts) 都支持这个参数）（[#6067](https://github.com/streamlit/streamlit/pull/6067)）。
- 👩‍💻 [st.code](/library/api-reference/text/st.code) 可以通过布尔参数 `line_numbers`（[#5756](https://github.com/streamlit/streamlit/issues/5756), [#6042](https://github.com/streamlit/streamlit/pull/6042)）选择性地在代码块的左侧显示行号。
- 可以通过设置 `anchor=False`（[#6158](https://github.com/streamlit/streamlit/pull/6158)）来关闭标题元素中的锚点。

**其他变更**

- 🐼 [st.table](/library/api-reference/data/st.table) 和 [st.dataframe](/library/api-reference/data/st.dataframe) 支持 `pandas.Period`，以及分类列中的数字和布尔类型（[#2547](https://github.com/streamlit/streamlit/issues/2547), [#5429](https://github.com/streamlit/streamlit/pull/5429), [#5329](https://github.com/streamlit/streamlit/issues/5392), [#6248](https://github.com/streamlit/streamlit/pull/6248))。
- 🕸️ 将`.webp`添加到允许的静态文件扩展名列表中 ([#6331](https://github.com/streamlit/streamlit/pull/6331))
- 🐞 修复bug：在WebSocket关闭时停止脚本执行以立即清除会话信息 ([#6166](https://github.com/streamlit/streamlit/issues/6166), [#6204](https://github.com/streamlit/streamlit/pull/6204)).
- 🐜 错误修复：更新了标签markdown的行为，不支持的元素将被移除，只渲染它们的子元素（文本内容）（[#5872](https://github.com/streamlit/streamlit/issues/5872), [#6036](https://github.com/streamlit/streamlit/issues/6036), [#6054](https://github.com/streamlit/streamlit/issues/6054), [#6163](https://github.com/streamlit/streamlit/pull/6163))。
- 🪲 错误修复: 在重新运行时不再推送浏览器历史状态，使用HTTPS加载`streamlit hello`中的外部资源，并为多页应用程序启用浏览器返回按钮功能([#5292](https://github.com/streamlit/streamlit/issues/5292), [#6266](https://github.com/streamlit/streamlit/pull/6266), [#6232](https://github.com/streamlit/streamlit/pull/6232))。感谢[whitphx](https://github.com/whitphx)！
- 🐝 修复bug：避免在非UTF-8终端上显示表情符号。([#2284](https://github.com/streamlit/streamlit/issues/2284), [#6088](https://github.com/streamlit/streamlit/pull/6088))。感谢[kcarnold](https://github.com/kcarnold)！
- 📁 错误修复: 覆盖了`react-dropzone`对于默认使用[File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API)的设置，使得`st.file_uploader`的文件选择对话框仅显示与`type`参数中包含的文件类型相对应的文件类型 ([#6176](https://github.com/streamlit/streamlit/issues/6176), [#6315](https://github.com/streamlit/streamlit/pull/6315)).
- 💾 修复Bug：使带有缓存修饰的函数上的 `.clear()` 方法正常工作（[#6310](https://github.com/streamlit/streamlit/issues/6310), [#6321](https://github.com/streamlit/streamlit/pull/6321)）。
- 🏃 修复Bug：`st.experimental_get_query_params` 不需要重新运行即可正常工作（[#6347](https://github.com/streamlit/streamlit/issues/6347), [#6348](https://github.com/streamlit/streamlit/pull/6348)）。感谢 [PaleNeutron](https://github.com/PaleNeutron)！
- 🐛 修复：`CachedStFunctionWarning` 提及了已弃用的 `suppress_st_warning` 而不是 `experimental_allow_widgets` ([#6216](https://github.com/streamlit/streamlit/issues/6216), [#6217](https://github.com/streamlit/streamlit/pull/6217))。

## **版本 1.20.0**

_发布日期：2023 年 3 月 9 日_

**重要变更**

- 🔐 新增了对配置SSL的支持，可以直接通过HTTPS来提供应用程序（[#5969](https://github.com/streamlit/streamlit/pull/5969)）。
- 通过`/?embed`和`/?embed_options`查询参数对应用程序的嵌入行为进行细粒度控制。在我们的[文档](/streamlit-community-cloud/get-started/deploy-an-app#embed-apps)中了解如何使用此功能（[#6011](https://github.com/streamlit/streamlit/pull/6011)，[#6019](https://github.com/streamlit/streamlit/pull/6019)）。
- ⚡ 默认情况下启用 `runner.fastReruns` [配置选项](/library/advanced-features/configuration#view-all-configuration-options)，使应用程序对用户交互更加响应快速 ([#6200](https://github.com/streamlit/streamlit/pull/6200))。

**其他更改**

- 🍔 通过删除最少使用的选项，简化了汉堡菜单 ([#6080](https://github.com/streamlit/streamlit/pull/6080))。
- 🖨️ 优化设计，确保应用程序在打印或保存为PDF时显示良好（[#6180](https://github.com/streamlit/streamlit/pull/6180)）。
- 🐞 错误修复：在`st.experimental_data_editor`中改进`dtypes`检查（[#6185](https://github.com/streamlit/streamlit/issues/6185)，[#6188](https://github.com/streamlit/streamlit/pull/6188)）。
- 🐛 错误修复：在不在列中时，正确定位`st.metric`的`help`工具提示（[#6168](https://github.com/streamlit/streamlit/pull/6168)）。
- 🪲 修复了从服务器的`ForwardMsgCache`中检索消息时出现的回归问题（[#6210](https://github.com/streamlit/streamlit/pull/6210)）。
- 🌀 修复了`st.cache_data`文档字符串中`show_spinner`参数现在将`str`作为支持的类型列出来的问题（[#6207](https://github.com/streamlit/streamlit/issues/6207)，[#6213](https://github.com/streamlit/streamlit/pull/6213)）。
- ⏱️ 使ping和websocket的超时时间更加宽容了（[#6212](https://github.com/streamlit/streamlit/pull/6212)）。
- 🗺️ `st.map`和`st.pydeck_chart`文档中指出Streamlit的Mapbox令牌将不会无限期工作（[#6143](https://github.com/streamlit/streamlit/pull/6143)）。

## **版本1.19.0**

_发布日期：2023年2月23日_

**亮点**

- ✂️ 推出 `st.experimental_data_editor`，一个允许您在类似表格的界面中编辑DataFrame和许多其他数据结构的小部件。在我们的[文档](/library/advanced-features/dataframes)和[博客文章](https://blog.streamlit.io/editable-dataframes-are-here/)中了解更多。

**其他变更**

- ✨ Streamlit的GitHub README界面有了新的外观（[#6016](https://github.com/streamlit/streamlit/pull/6016)）。
- 🌚 提升了在暗模式下样式化数据框单元格的可读性（[#6060](https://github.com/streamlit/streamlit/issues/6060)，[#6098](https://github.com/streamlit/streamlit/pull/6098)）。
- 🐛 修复了应用在最新版本的Safari中无法工作以及在阻止第三方Cookie的Chrome中无法工作的问题（[#6092](https://github.com/streamlit/streamlit/issues/6092), [#6094](https://github.com/streamlit/streamlit/pull/6094), [#6087](https://github.com/streamlit/streamlit/issues/6087), [#6100](https://github.com/streamlit/streamlit/pull/6100)).
- 🐞 错误修复：在“清除缓存”对话框和错误消息中引用了新的缓存原语（[#6082](https://github.com/streamlit/streamlit/pull/6082)，[#6128](https://github.com/streamlit/streamlit/pull/6128)）。
- 🐝 错误修复：正确缓存类成员函数和实例方法（[#6109](https://github.com/streamlit/streamlit/issues/6109)，[#6114](https://github.com/streamlit/streamlit/pull/6114)）。
- 🐜 错误修复：修复了`st.metric`工具提示位置的回归问题（[#6093](https://github.com/streamlit/streamlit/issues/6093), [#6129](https://github.com/streamlit/streamlit/pull/6129)）。
- 🪲 错误修复：允许数据框、图表等在展开器中显示全屏按钮（[#6083](https://github.com/streamlit/streamlit/pull/6083), [#6148](https://github.com/streamlit/streamlit/pull/6148)）。

## **版本 1.18.0**

发布日期：2023年2月9日

**亮点**

- 🎊 介绍了`@st.cache_data`和`@st.cache_resource`两个新的缓存命令，用于替代`st.cache`！请查看我们的[博客文章](https://blog.streamlit.io/p/c0a90231-9848-47ec-a40c-ad4a344e4de1/)和[文档](/library/advanced-features/caching)获取更多信息。

**重要变更**

- 🪆 `st.columns`在应用程序的主区域支持最多一级列嵌套（即列内嵌列）。
- ⏳ `st.progress`函数支持使用`text`参数在进度条上方添加显示信息。
- ↔️ `st.button`函数有一个可选的`use_container_width`参数，可以让按钮横跨整个容器的宽度。
- 🐍 我们正式支持Python 3.11版本。
- 🖨️ 通过应用程序的汉堡菜单中的“打印”选项，将您的应用程序保存为PDF文件。
- 🛎️ 应用程序可以通过`enableStaticServing`配置选项提供小型的静态媒体文件。请参阅我们的[文档](/library/advanced-features/static-file-serving)了解如何使用此功能，并查看我们的演示[应用程序](https://static-file-serving.streamlit.app/)以获得示例。

**其他更改**

- 🏁 所有的Streamlit端点（包括`/healthz`）已经重命名，以保持一致的模式，并避免与GCP（特别是Cloud Run和App Engine）的保留端点发生冲突（[#5534](https://github.com/streamlit/streamlit/pull/5534)）。
- ⚡ 当多个会话同时访问未计算的缓存值时，改进了缓存性能（[#6017](https://github.com/streamlit/streamlit/pull/6017)）。
- 🚧 当 `client.showErrorDetails` 配置选项设置为 `True` 时，Streamlit 只在浏览器中显示弃用警告。无论是否在浏览器中显示，弃用警告始终会被记录在控制台中（[#5945](https://github.com/streamlit/streamlit/pull/5945)）。
- 🏓 重构了 `st.dataframe` 内部以改进数据框处理和转换，例如检测更多类型，将键值字典转换为数据框等等 ([#6026](https://github.com/streamlit/streamlit/pull/6026), [#6023](https://github.com/streamlit/streamlit/pull/6023)).
- 💽 当传递不支持的 Markdown 元素时，小部件标签的行为已经有了文档化 ([#5978](https://github.com/streamlit/streamlit/pull/5978)).
- 📊 错误修复：Plotly改进 - 将多个前端依赖项（包括Plotly）升级到最新版本，以正确重绘缓存的图表，使Plotly地图动画正常工作，并允许用户在使用Streamlit主题时更新图形布局（[#5885](https://github.com/streamlit/streamlit/pull/5885)，[#5967](https://github.com/streamlit/streamlit/pull/5967)，[#6055](https://github.com/streamlit/streamlit/pull/6055))。
- 📶 Bug修复：允许浏览器标签在短暂断开连接时（由于网络中断、负载均衡器超时等原因）避免丢失所有状态（[#5856](https://github.com/streamlit/streamlit/pull/5856)）。
- 📱 Bug修复：当`st.selectbox`和`st.multiselect`的选项少于10个时，移动设备上的键盘会被隐藏（[#5979](https://github.com/streamlit/streamlit/pull/5979)）。
- 🐝 修复了 `st.metric`、`st.multiselect`、`st.tabs` 和菜单项的设计调整，以防止标签溢出和滚动问题，特别是在小视口大小下（[#5933](https://github.com/streamlit/streamlit/pull/5933), [#6034](https://github.com/streamlit/streamlit/pull/6034)）。
- 🐞 修复了在 `st.set_page_config` 中加载页面图标的 Twemoji URL，使其正常工作（[#5943](https://github.com/streamlit/streamlit/pull/5943)）。
- ✍️ 更多的类型提示 ([#5986](https://github.com/streamlit/streamlit/pull/5986))。感谢 [harahu](https://github.com/harahu)!

## **版本 1.17.0**

发布日期：2023年1月12日

**重要变更**

- 🪄 [`@st.experimental_singleton`](/library/api-reference/performance/st.experimental_singleton#validating-the-cache) 支持一个可选的 `validate` 参数，用于接受一个验证函数来验证缓存数据，并在每次访问缓存值时调用。
- 💾 [`@st.experimental_memo`](https://docs.streamlit.io/library/api-reference/performance/st.experimental_memo)的`persist`参数还可以接受布尔值。

**其他变更**

- 📟 多页面应用将`__init__.py`从页面选择器中排除（[#5890](https://github.com/streamlit/streamlit/pull/5890)）。
- 📐 嵌入式应用的iframe可以动态调整其高度（[#5894](https://github.com/streamlit/streamlit/pull/5894)）。
- 🐞 修复：范围滑块的缩略图值将考虑容器宽度（[#5913](https://github.com/streamlit/streamlit/pull/5913)）。
- 🪲 修复：所有Streamlit命令的文档字符串示例都包含相关的导入以使其可复现（[#5877](https://github.com/streamlit/streamlit/pull/5877)）。

## **版本 1.16.0**

发布日期：2022 年 12 月 14 日

**亮点**

- 👩‍🎨为Altair、Plotly和Vega-Lite图表引入了一个新的Streamlit主题！请查看我们的[博文](https://blog.streamlit.io/a-new-streamlit-theme-for-altair-and-plotly/)获取更多信息。
- 🎨现在，Streamlit在所有支持Markdown的命令中都支持彩色文本，包括`st.markdown`、`st.header`等。请在我们的[文档](/library/api-reference/text/st.markdown)中了解更多信息。

**重要变更**

- 使用 `st.experimental_memo` 或 `st.experimental_singleton` 缓存的函数可以包含 Streamlit 媒体元素和表单。
- 所有接受 pandas DataFrames 作为输入的 Streamlit 命令也支持 Snowpark 和 PySpark DataFrames。
- 使用 `label_visibility` 参数，[st.checkbox](/library/api-reference/widgets/st.checkbox) 和 [st.metric](/library/api-reference/data/st.metric) 可以自定义隐藏标签的方式。

**其他变更**

- 🗺️ `st.map`改进：支持大写列和更好的异常消息（[#5679](https://github.com/streamlit/streamlit/pull/5679)，[#5792](https://github.com/streamlit/streamlit/pull/5792))。
- 🐞 错误修复：`st.plotly_chart`遵循图表的高度属性和`use_container_width`参数（[#5779](https://github.com/streamlit/streamlit/pull/5779))。
- 🪲 修复错误：所有带有 `icon` 参数的命令，例如 [st.error](/library/api-reference/status/st.error)，[st.warning](/library/api-reference/status/st.warning) 等，可以包含带有变体选择器的表情符号（[#5583](https://github.com/streamlit/streamlit/pull/5583)）。
- 🐝 修复错误：当调整浏览器窗口大小时，防止 `st.camera_input` 抖动（[#5661](https://github.com/streamlit/streamlit/pull/5711)）。
- 🐜 错误修复：更新异常布局以避免堆栈跟踪溢出（[#5700](https://github.com/streamlit/streamlit/pull/5700)）。

## **版本 1.15.0**

发布日期：2022年11月17日

**重要更改**

- 💅 小部件标签可以包含内联的Markdown。有关更多信息，请参阅我们的[文档](https://docs.streamlit.io/library/api-reference/widgets)和演示[应用程序](https://markdown-labels.streamlit.app/)。
- 🎵 [`st.audio`](/library/api-reference/media/st.audio) 现在支持使用关键字参数`sample_rate`将音频数据作为NumPy数组进行播放。
- 🔁 使用`st.experimental_memo`或`st.experimental_singleton`缓存的函数可以包含使用`experimental_allow_widgets`参数的Streamlit小部件。这允许缓存复选框、滑块、单选按钮等小部件！

**其他变更**

- 👩‍🎨 对滑块进行设计调整，以防止抖动（[#5612](https://github.com/streamlit/streamlit/pull/5612)）。
- 🐛 修复Bug：标题中的链接是红色而不是蓝色（[#5609](https://github.com/streamlit/streamlit/pull/5609)）。
- 🐞 修复Bug：在退出全屏时正确调整Plotly图表的大小（[#5645](https://github.com/streamlit/streamlit/pull/5645)）。
- 🐝 修复Bug：不要意外触发`st.balloons`和`st.snow`（[#5401](https://github.com/streamlit/streamlit/pull/5401)）。

## **版本 1.14.0**

_发布日期：2022 年 10 月 27 日_

**亮点**

- 🎨 `st.button` 和 `st.form_submit_button` 支持使用 `type` 关键字参数指定按钮为“主要”（以增加强调）或“次要”（作为普通按钮）。

**重要变更**

- 🤏 `st.multiselect` 现在有一个关键字参数 `max_selections`，用于限制一次可以选择的选项数。
- 📄 `st.form_submit_button`现在有一个`disabled`参数，可以去除交互性。

**其他变更**

- 🏓 `st.dataframe`和`st.table`现在接受分类间隔作为输入 ([#5395](https://github.com/streamlit/streamlit/pull/5395)).
- ⚡ 对Plotly图表进行了性能优化 ([#5542](https://github.com/streamlit/streamlit/pull/5542)).
- 🪲 修复了一个bug: `st.download_button`支持非Latin1字符的文件名 ([#5465](https://github.com/streamlit/streamlit/pull/5465)).
- 🐞 修复：允许`st.image`以GIF格式渲染本地GIF，而不是作为静态PNG（[#5438](https://github.com/streamlit/streamlit/pull/5438)）。
- 📱 多页面应用程序侧边栏的设计微调（[#5538](https://github.com/streamlit/streamlit/pull/5538)，[#5445](https://github.com/streamlit/streamlit/pull/5445)，[#5559](https://github.com/streamlit/streamlit/pull/5559)）。
- 📊 内置图表轴配置的改进（[#5412](https://github.com/streamlit/streamlit/pull/5412)）。
- 🔧 改进备忘录和单例：支持`show_spinner`的文本值，将`datetime.timedelta`对象作为`ttl`参数值，正确哈希PIL图像和`Enum`类，在返回未计算的数据帧时显示更好的错误消息（[#5447](https://github.com/streamlit/streamlit/pull/5447)，[#5413](https://github.com/streamlit/streamlit/pull/5413)，[#5504](https://github.com/streamlit/streamlit/pull/5504)，[#5426](https://github.com/streamlit/streamlit/pull/5426)，[#5515](https://github.com/streamlit/streamlit/pull/5515))。
- 🔍 使用 `st.map` 和 `st.pydeck_chart` 创建的地图中的缩放按钮根据应用程序的主题使用浅色或深色样式 ([#5479](https://github.com/streamlit/streamlit/pull/5479))。
- 🗜 可以从新的 "internal" (即：可能会发生更改而无需弃用) API 中获取当前会话的传入 WebSocket 请求的 Websocket headers ([#5457](https://github.com/streamlit/streamlit/pull/5457))。
- 📝 改进了首次安装和使用Streamlit时打印的文本（[#5473](https://github.com/streamlit/streamlit/pull/5473))。

## **版本 1.13.0**

发布日期：2022 年 9 月 22 日

**重要变更**

- 🏷 小部件可以使用`label_visibility`参数自定义隐藏标签的方式。
- 🔍 `st.map` 默认添加缩放按钮到地图中。
- ↔️ `st.dataframe` 支持`use_container_width`参数以横向拉伸至容器的全部宽度。
- 🪄 改进了 `st.dataframe` 的大小调整功能：列宽计算考虑了列头，支持在列头之间双击自动调整大小，更好的全屏支持，并修复了 `width` 参数的问题。

**其他变更**

- ⌨️ `st.time_input` 允许仅使用键盘进行输入 ([#5194](https://github.com/streamlit/streamlit/pull/5194))。
- 💿 `st.memo` 在使用 `ttl` 和 `persist` 关键字参数时会给用户发出警告 ([#5032](https://github.com/streamlit/streamlit/pull/5032))。
- 🔢 `st.number_input` 在重新运行后返回一致的类型 ([#5359](https://github.com/streamlit/streamlit/pull/5359))。
- 🚒 `st.sidebar` UI 修复，包括修复 Firefox 浏览器中的滚动条问题 ([#5157](https://github.com/streamlit/streamlit/pull/5157), [#5324](https://github.com/streamlit/streamlit/pull/5324))。
- 👩‍💻 改进使用指标以指导API开发。
- ✍️ 更多类型提示！（[#5191](https://github.com/streamlit/streamlit/pull/5191), [#5192](https://github.com/streamlit/streamlit/pull/5192), [#5242](https://github.com/streamlit/streamlit/pull/5242), [#5243](https://github.com/streamlit/streamlit/pull/5243), [#5244](https://github.com/streamlit/streamlit/pull/5244), [#5245](https://github.com/streamlit/streamlit/pull/5245), [#5246](https://github.com/streamlit/streamlit/pull/5246)) 感谢 [harahu](https://github.com/harahu)！

## **版本 1.12.0**

发布日期：2022年8月11日

**亮点**

- 📊 内置图表（例如 `st.line_chart`）有了全新的外观和参数 `x` 和 `y`！更多信息请查看我们的[博客文章](https://blog.streamlit.io/built-in-charts-get-a-new-look-and-parameters/)。

**重要变更**

- ⏯ 使用 `st.experimental_memo` 或 `st.experimental_singleton` 缓存的函数现在可以包含静态的 `st` 命令。这使得可以缓存文本、图表、数据框等内容！
- ↔️ 通过拖放可以调整侧边栏的大小。
- ☎️ `st.info`、`st.success`、`st.error`和`st.warning`进行了重新设计，并新增了一个关键字参数：`icon`。

**其他变更**

- 🎚️ `st.select_slider` 现在可以正确处理所有浮点数（[#4973](https://github.com/streamlit/streamlit/pull/4973)，[#4978](https://github.com/streamlit/streamlit/pull/4978)）。
- 🔢 `st.multi_select` 可以接受枚举类型的值（[#4987](https://github.com/streamlit/streamlit/pull/4987)）。
- 🍊 `st.slider` 现在可以通过 `st.session_state` 设置范围值（[#5007](https://github.com/streamlit/streamlit/pull/5007)）。
- 🎨 `st.progress` 进行了重新设计（[#5011](https://github.com/streamlit/streamlit/pull/5011)，[#5086](https://github.com/streamlit/streamlit/pull/5086)）。
- 🔘 `st.radio` 更好地处理类似列表的数据帧（[#5021](https://github.com/streamlit/streamlit/pull/5021)）。
- 🧞‍♂️ `st.cache` 现在能够正确处理 JSON 文件了（[#5023](https://github.com/streamlit/streamlit/pull/5023)）。
- ⚓️ 当设置了 `anchor` 参数时，标题现在可以渲染为 markdown 了（[#5038](https://github.com/streamlit/streamlit/pull/5038)）。
- 🗻 `st.image` 现在可以从 Inkscape 中加载 SVG 文件了（[#5040](https://github.com/streamlit/streamlit/pull/5040)）。
- 🗺️ `st.map`和`st.pydeck_chart`根据应用程序的主题使用浅色或深色样式（[#5074](https://github.com/streamlit/streamlit/pull/5074), [#5108](https://github.com/streamlit/streamlit/pull/5108)）。
- 🎈 `st.balloons`和`st.snow`下方的元素点击不再被阻止（[#5098](https://github.com/streamlit/streamlit/pull/5098)）。
- 🔝 嵌入式应用程序的顶部填充较小（[#5111](https://github.com/streamlit/streamlit/pull/5111)）。
- 💅 调整了小部件、图表和数据框的填充和对齐方式（[#4995](https://github.com/streamlit/streamlit/pull/4995), [#5061](https://github.com/streamlit/streamlit/pull/5061), [#5081](https://github.com/streamlit/streamlit/pull/5081))。
- ✍️ 更多的类型提示！（[#4926](https://github.com/streamlit/streamlit/pull/4926), [#4932](https://github.com/streamlit/streamlit/pull/4932), [#4933](https://github.com/streamlit/streamlit/pull/4933))

## **版本 1.11.0**

_发布日期：2022年7月14日_

**亮点**

- 🗂 引入`st.tabs`，在您的应用程序中使用选项卡容器。请参阅我们的[文档](/library/api-reference/layout/st.tabs)以了解如何使用此功能。

**重要变更**

- ℹ️ `st.metric`支持使用`help`关键字参数显示工具提示。
- 🚇 `st.columns`支持使用`gap`关键字参数设置列之间的间距大小。

**其他变更**

- 💅 对 `st.selectbox`, `st.expander`, `st.spinner` 进行了设计调整 ([#4801](https://github.com/streamlit/streamlit/pull/4801)).
- 📱 在移动设备上，当用户从导航菜单中选择一个页面时，侧边栏将关闭 ([#4851](https://github.com/streamlit/streamlit/pull/4841)).
- 🧠 `st.memo` 支持数据类! ([#4850](https://github.com/streamlit/streamlit/pull/4850))
- 🏎 修复了一种竞态条件导致快速交互时破坏小部件状态的错误（[#4882](https://github.com/streamlit/streamlit/pull/4882)）。
- 🏓 `st.table`现在在放置在列和展开器内时可以滚动显示溢出内容（[#4934](https://github.com/streamlit/streamlit/pull/4934)）。
- 🐍 类型：更多更新的类型注释在Streamlit中！([#4808](https://github.com/streamlit/streamlit/pull/4808), [#4809](https://github.com/streamlit/streamlit/pull/4809), [#4856](https://github.com/streamlit/streamlit/pull/4856))

## **版本 1.10.0**

发布日期：2022年6月2日

**亮点**

- 📖 引入原生支持多页面应用程序！请查看我们的[博客文章](https://blog.streamlit.io/introducing-multipage-apps)并尝试我们的新`streamlit hello`。

**重要更改**

- ✨ `st.dataframe` 已经重新设计。
- 🔘 `st.radio` 添加了 `horizontal` 参数，可以水平显示选项。
- ⚠️ Streamlit Community Cloud 将支持更丰富的异常格式化。
- 🏂 使用 `st.experimental_user` 可以在私有应用程序中获取用户信息。

**其他变更**

- 📊 升级Vega-Lite库以支持更多的交互式图表改进。请查看他们的[发布说明](https://github.com/vega/vega-lite/releases)以获取更多信息。([#4751](https://github.com/streamlit/streamlit/pull/4751)).
- 📈 `st.vega_lite_chart`将响应更新，特别是响应输入小部件的更新。([#4736](https://github.com/streamlit/streamlit/pull/4736)).
- 💬 使用长文本的`st.markdown`将始终换行。([#4696](https://github.com/streamlit/streamlit/pull/4696)).
- 📦 支持[PDM](https://pdm.fming.dev/)（[#4724](https://github.com/streamlit/streamlit/pull/4724)）.
- ✍️ 类型注解：更新了Streamlit中的类型注解！（[#4679](https://github.com/streamlit/streamlit/pull/4679), [#4680](https://github.com/streamlit/streamlit/pull/4680), [#4681](https://github.com/streamlit/streamlit/pull/4681), [#4682](https://github.com/streamlit/streamlit/pull/4682), [#4683](https://github.com/streamlit/streamlit/pull/4683), [#4684](https://github.com/streamlit/streamlit/pull/4684), [#4685](https://github.com/streamlit/streamlit/pull/4685), [#4686](https://github.com/streamlit/streamlit/pull/4686), [#4687](https://github.com/streamlit/streamlit/pull/4687), [#4688](https://github.com/streamlit/streamlit/pull/4688), [#4690](https://github.com/streamlit/streamlit/pull/4690), [#4703](https://github.com/streamlit/streamlit/pull/4703), [#4704](https://github.com/streamlit/streamlit/pull/4704), [#4705](https://github.com/streamlit/streamlit/pull/4705), [#4706](https://github.com/streamlit/streamlit/pull/4706), [#4707](https://github.com/streamlit/streamlit/pull/4707), [#4708](https://github.com/streamlit/streamlit/pull/4708), [#4710](https://github.com/streamlit/streamlit/pull/4710), [#4723](https://github.com/streamlit/streamlit/pull/4723), [#4733](https://github.com/streamlit/streamlit/pull/4733))）。

## **版本 1.9.0**

_发布日期：2022 年 5 月 4 日_

**重要变更**

- 🪗 `st.json` 现在支持一个关键字参数 `expanded`，用于指定 JSON 是否应该默认展开（默认为 `True`）。
- 🏃‍♀️ 通过减少每次脚本运行中的冗余工作，进一步提高性能。

**其他变更**

- 🏇 当 `disabled` 被设置或取消时，小部件将保持其值（[#4527](https://github.com/streamlit/streamlit/pull/4527)）。
- 🧪 引入了实验性功能，使用配置项 `runner.fastReruns` 可以提高重新运行速度。请参考[#4628](https://github.com/streamlit/streamlit/pull/4628)了解启用此功能时已知的问题。
- 🗺️ DataFrame 时间戳支持 UTC 偏移（除了时区表示法）（[#4669](https://github.com/streamlit/streamlit/pull/4669)）。

## **版本 1.8.0**

_发布日期：2022年3月24日_

**重要更新**

- 🏃‍♀️ 数据框架应该会看到性能改进（[#4463](https://github.com/streamlit/streamlit/pull/4463))。

**其他变更**

- 🕰 `st.slider` 通过在后端删除时区转换来更好地处理时区（[#4348](https://github.com/streamlit/streamlit/pull/4358))。
- 👩‍🎨 对我们的标题进行了设计改进（[#4496](https://github.com/streamlit/streamlit/pull/4496))。

## **版本 1.7.0**

发布日期：2022年3月3日

**亮点**

- 介绍 `st.snow`，庆祝我们被Snowflake收购！在[我们的博客文章](https://blog.streamlit.io/snowflake-to-acquire-streamlit/)中获取更多信息。

## **版本 1.6.0**

_发布日期：2022年2月24日_

**其他变更**

- 🗜 WebSocket压缩默认已禁用，这将提高处理大型数据帧的CPU和延迟性能。如果您发现增加的网络流量对性能影响更大，您可以使用 `server.enableWebsocketCompression` 配置选项重新启用它。
- ☑️ 🔘 单选框和复选框在键盘导航时改善了焦点（[#4308](https://github.com/streamlit/streamlit/pull/4308)）。

## **版本1.5.0**

_发布日期：2022年1月27日_

**重要更改**

- 🌟 网站图标默认为PNG格式，以支持透明度（[#4272](https://github.com/streamlit/streamlit/pull/4272)）。
- 🚦 选择滑块部件现在具有`disabled`参数，可以取消交互性（完成所有部件）（[#4314](https://github.com/streamlit/streamlit/pull/4314)）。

**其他变更**

- 🔤 改进了我们的Markdown库，以更好地支持HTML（特别是嵌套HTML）（[#4221](https://github.com/streamlit/streamlit/pull/4221)）。
- 📖 当存在多个展开器时，展开器在维护其展开状态时更加稳定。（[#4290](https://github.com/streamlit/streamlit/pull/4290)）
- 🗳 改进的文件上传器和相机输入器，仅在必要时调用其`on_change`处理程序。（[#4270](https://github.com/streamlit/streamlit/pull/4270)）

## **版本 1.4.0**

发布日期：2022 年 1 月 13 日

**亮点**

- 📸 引入`st.camera_input`，可以直接从相机上传图片。

**重要变更**

- 🚦 现在小组件具有`disabled`参数，可以去除交互功能。
- 🚮 通过在缓存函数上使用`clear()`方法，可以以编程方式清除`st.experimental_memo`和`st.experimental_singleton`。
- 📨 现在开发人员可以配置消息的最大大小，以适应Streamlit应用程序中的较大消息。请参阅`server.maxMessageSize`。
- 🐍 我们正式支持Python 3.10。

**其他更改**

- 😵‍💫 调用 `threading.current_thread()` 的 `str` 或 `repr` 不会引发 RecursionError ([#4172](https://github.com/streamlit/streamlit/issues/4172))。
- 📹 当用户取消录制权限时，优雅地停止屏幕录制 ([#4180](https://github.com/streamlit/streamlit/pull/4180))。
- 🌇 通过使用更高质量的图像双线性重采样算法来更好地缩放图像 ([#4159](https://github.com/streamlit/streamlit/pull/4159))。

## 版本 1.3.0

_发布日期：2021年12月16日_

**重要变更**

- 💯 支持在`st.metric`中使用NumPy值。
- 🌐 在PyDeck中支持网格图层。
- 📊 更新Plotly图表版本以支持最新功能。
- 🏀 `st.spinner`元素具有可视化的动画加载符号。
- 🍰 `st.caption`支持在文本中使用HTML，使用`unsafe_allow_html`参数。

**其他变更**

- 🪲 修复了一个bug：允许使用`st.session_state`设置`number_input`值而不会出现警告（[#4047](https://github.com/streamlit/streamlit/pull/4047)）。
- 🪲 缺陷修复：修复宽模式下页脚对齐问题（[#4035](https://github.com/streamlit/streamlit/pull/4035)）。
- 🐞 缺陷修复：在容器（列、展开器等）中更好地支持Graphviz和Bokeh图表（[#4039](https://github.com/streamlit/streamlit/pull/4039)）。
- 🐞 缺陷修复：支持Vega-Lite中的内联数据值（[#4070](https://github.com/streamlit/streamlit/pull/4070)）。
- ✍️ 类型：更新实验性的memo和singleton装饰器的类型注解。
- ✍️ 类型：对`st.selectbox`、`st.select_slider`、`st.radio`、`st.number_input`和`st.multiselect`进行了改进的类型注释。

## 版本 1.2.0

_发布日期：2021年11月11日_

**重要变更**

- ✏️ `st.text_input`和`st.text_area`现在具有`placeholder`参数，用于在字段为空时显示文本。
- 📏 查看者现在可以调整`st.text_area`中的输入框大小。
- 📁 当子目录中的文件发生更改时，Streamlit可以自动重新加载。
- 🌈我们已将Bokeh支持升级到2.4.1！我们建议您将Bokeh库更新到2.4.1以保持功能正常。从现在开始，如果Bokeh版本不匹配，我们将通过错误提示向您提醒。
- 🔒开发人员可以通过属性表示法（例如`st.secrets.key`而不是`st.secrets["key"]`）来访问密钥，就像会话状态一样。
- ✍️ 根据[PEP 561](https://mypy.readthedocs.io/en/stable/installed_packages.html)发布类型注释。用户现在在运行mypy时可以得到Streamlit的类型注释（[#4025](https://github.com/streamlit/streamlit/pull/4025)）。

**其他变更**

- 👀 视觉修复 ([#3863](https://github.com/streamlit/streamlit/pull/3863), [#3995](https://github.com/streamlit/streamlit/pull/3995), [#3926](https://github.com/streamlit/streamlit/pull/3926), [#3975](https://github.com/streamlit/streamlit/pull/3975)).
- 🍔 修复汉堡菜单 ([#3968](https://github.com/streamlit/streamlit/pull/3968)).
- 🖨️ 支持打印会话状态 ([#3970](https://github.com/streamlit/streamlit/pull/3970))。

## 版本 1.1.0

_发布日期：2021年10月21日_

**亮点**

- 🧠 内存改进：Streamlit应用程序现在在运行时分配的内存大大减少。

**重要更改**

- ♻️ 现在当`secrets.toml`文件的内容发生更改时，应用程序会自动重新运行（之前需要手动刷新页面）。

**其他更改**

- 🔗 将一些链接重定向到我们的[全新文档网站](https://docs.streamlit.io/)，例如在异常中的链接。
- 🪲 修复了一个问题：允许使用会话状态初始化范围滑块（[#3586](https://github.com/streamlit/streamlit/issues/3586)）。
- 🐞 修复了一个问题：在使用`add_rows`和`datetime`索引时刷新图表（[#3653](https://github.com/streamlit/streamlit/issues/3653)）。
- ✍️ 在我们的代码库中添加了一些类型注释（[#3908](https://github.com/streamlit/streamlit/issues/3908)）。

## 版本 1.0.0

_发布日期：2021年10月5日_

**亮点**

- 🎈宣布Streamlit 1.0发布！要了解更多信息，请查看我们的[1.0博客文章](https://blog.streamlit.io/announcing-streamlit-1-0/)。

**其他变更**

- 🐞修复了使用`df.dtypes`显示DataFrame数据类型时，在使用Arrow时失败的问题（[#3709](https://github.com/streamlit/streamlit/issues/3709)），图像标题保持在图像宽度内可读（[#3530](https://github.com/streamlit/streamlit/issues/3530)）。

## 版本0.89.0

_发布日期：2021年9月22日_

**亮点**

- 💰介绍`st.experimental_memo`和`experimental_singleton`，这是一种新的缓存原语！请参阅[我们的博客文章](https://blog.streamlit.io/new-experimental-primitives-for-caching/)。
- 🍔 Streamlit允许开发人员配置汉堡菜单，使其更加用户友好。

**重要变更**

- 💅我们更新了UI，采用了更加精美的字体。
- 🎨现在，当将主题对象发送到自定义组件时，我们支持`theme.base`。
- 🧠我们修改了会话状态，以便在任何参数发生变化时重置小部件，即使它们提供了键。
  - 一些小部件的行为可能发生了变化，但我们认为这个改变是最合理的。我们在[我们的文档](/library/advanced-features/widget-semantics)中添加了一个描述它们如何工作的部分。

**其他变更**

- 🐞 Bug修复：支持从URL下载SVG图像（[#3809](https://github.com/streamlit/streamlit/pull/3809)），以及不以`<svg>`标签开头的SVG图像（[#3789](https://github.com/streamlit/streamlit/pull/3789)）。

## 版本 0.88.0

_发布日期：2021年9月2日_

**亮点**

- ⬇️ 引入`st.download_button`，一个新的按钮小部件，用于方便地下载文件。

**重要更改**

- 🛑 我们对Streamlit社区云进行了更改，以改善已屏蔽异常的体验。当`client.showErrorDetails=true`时，异常会显示错误类型和Traceback，但屏蔽实际错误文本以防止数据泄漏。

## 版本0.87.0

_发布日期：2021年8月19日_

**亮点**

- 🔢 引入了`st.metric`，用于显示关键绩效指标的API。请查看[演示应用程序](https://streamlit-release-demos-0-87streamlit-app-0-87-rfzphf.streamlit.app/)，展示了该功能的使用。

**其他变更**

- 🐞 **错误修复**：文件上传器在关闭展开器时保留状态（[#3557](https://github.com/streamlit/streamlit/issues/3557)），`st.empty` 中的 setIn 错误（[#3659](https://github.com/streamlit/streamlit/issues/3659)），文档中缺少 IFrame 嵌入（[#3706](https://github.com/streamlit/streamlit/issues/3706)），修复写入某些 PNG 文件时的错误（[#3597](https://github.com/streamlit/streamlit/issues/3597)）。

## 版本 0.86.0

_发布日期：2021 年 8 月 5 日_

**亮点**

- 🎓 我们的布局基元已经从 beta 版本中毕业了！现在您可以在不使用 `beta_` 前缀的情况下使用 `st.columns`、`st.container` 和 `st.expander`。

**重要变更**

- 📱 当使用 `st.columns` 时，在视口大小小于 640px 时，列会垂直堆叠，以确保在较小的视口上布局一致且更清晰。([#3594](https://github.com/streamlit/streamlit/issues/3594))。

**其他变更**

- 🐞 **错误修复**：修复了 `st.date_input` 在为空时崩溃的问题（[#3194](https://github.com/streamlit/streamlit/issues/3194)），使用 utf-8 打开文件（[#3022](https://github.com/streamlit/streamlit/issues/3022)），`st.select_slider` 在交互后重置其状态（[#3600](https://github.com/streamlit/streamlit/issues/3600)）。

## 版本 0.85.0

_发布日期：2021 年 7 月 22 日_

**亮点**

- 🏹 现在，Streamlit在将数据帧从Streamlit服务器发送到前端时使用[Apache Arrow](https://arrow.apache.org)进行序列化。请查看我们的[博客文章](https://blog.streamlit.io/)。
- （希望继续使用传统数据帧序列化的用户可以在其`config.toml`中将`dataFrameSerialization`配置选项设置为`"legacy"`）。

**其他变更**

- 🐞 修复：无响应的pydeck示例（[#3395](https://github.com/streamlit/streamlit/issues/3395)），JSON解析错误消息（[#2324](https://github.com/streamlit/streamlit/issues/2324)），工具提示渲染（[#3300](https://github.com/streamlit/streamlit/issues/3300)），颜色选择器在Streamlit Sharing上不起作用（[#2689](https://github.com/streamlit/streamlit/issues/2689)）。

## 版本 0.84.0

_发布日期：2021年7月1日_

**亮点**

- 🧠 引入`st.session_state`和小部件回调函数，使您能够向应用程序添加状态性。查看[博客文章](http://blog.streamlit.io/session-state-for-streamlit/)了解更多信息。

**显著变更**

- 🪄 `st.text_input`现在具有`autocomplete`参数，以允许使用密码管理器

**其他变更**

- 使用`st.set_page_config`来分配页面标题不再将“Streamlit”附加到该标题上([#3467](https://github.com/streamlit/streamlit/pull/3467))
- NumberInput：当小部件已达到最大（或最小）值时，禁用加号/减号按钮（[#3493](https://github.com/streamlit/streamlit/pull/3493)）

## 版本 0.83.0

_发布日期：2021 年 6 月 17 日_

**亮点**

- 🛣️ 更新 Streamlit 文档，包括逐步指南，演示如何将 Streamlit 应用程序连接到各种数据库和 API

**重要变更**

- 📄 `st.form` 现在有一个 `clear_on_submit` 参数，在提交表单时“重置”所有表单小部件。

**其他变更**

- 修复了有关文件编码的错误（[#3320](https://github.com/streamlit/streamlit/issues/3220), [#3108](https://github.com/streamlit/streamlit/issues/3108), [#2731](https://github.com/streamlit/streamlit/issues/2731))

## 版本 0.82.0

_发布日期：2021年5月13日_

**重要变更**

- ♻️ 通过在脚本运行之间强制进行垃圾回收，改进了内存管理。

## 版本 0.81.1

_发布日期：2021年4月29日_

**亮点**

- 📝 引入 `st.form` 和 `st.form_submit_button`，允许您批量输入小部件。请查看我们的[博客文章](http://blog.streamlit.io/introducing-submit-button-and-forms)
- 🔤 引入 `st.caption`，这样您就可以在应用程序的任何位置添加解释性文本。
- 🎨 更新了主题设置，包括能够构建从任何默认主题继承的主题。
- 🚀 对从应用程序菜单部署到Streamlit共享的体验进行了改进。

**其他更改**

- 支持在自定义组件中处理二进制文件（[#3144](https://github.com/streamlit/streamlit/pull/3144)）

## 版本 0.80.0

_发布日期：2021年4月8日_

**亮点**

- 🔐 Streamlit 现在支持在部署到 Streamlit Sharing 的应用程序中进行密钥管理！
- ⚓️ 标题和标题现在带有自动生成的锚链接。只需将鼠标悬停在任何标题上，然后点击🔗获取链接！

**其他变更**

- 为自定义组件添加了`allow-downloads`功能（[#3040](https://github.com/streamlit/streamlit/issues/3040)）
- 修复了暗色主题下的Markdown表格显示问题（[#3020](https://github.com/streamlit/streamlit/issues/3020)）
- 改进了自定义主题对话框中的颜色选择器小部件（[#2970](https://github.com/streamlit/streamlit/issues/2970)）

## 版本 0.79.0

_发布日期：2021 年 3 月 18 日_

**亮点**

- 🌈 引入自定义主题的支持。查看我们的[博客文章](http://blog.streamlit.io/introducing-theming/)。
- 🌚 此版本还引入了暗黑模式！
- 🛠️ 对所有输入小部件添加了工具提示的支持。

**其他变更**

- 修复了有关文件编码（[#1936](https://github.com/streamlit/streamlit/issues/1936)，[#2606](https://github.com/streamlit/streamlit/issues/2606)）和缓存函数（[#2728](https://github.com/streamlit/streamlit/issues/2728)）的错误。

## 版本 0.78.0

_发布日期: 2021年3月4日_

**特性**

- 如果您正在使用Streamlit for Teams beta版，我们对密码保护功能进行了一些更新。请查阅beta版文档了解更多信息！
- 数据框现在会显示所有DateTime和Time列的时区，并显示应用时区后的时间，而不是以UTC时间显示

**重要Bug修复**

- 对`st.beta_columns`中的列对齐进行了各种改进
- 移除了`st.image`中已经被弃用很久的`format`参数，并替换为`output_format`参数。

## 版本 0.77.0

发布日期：2021年2月23日

**新功能**

- 添加了一个新的配置选项`client.showErrorDetails`，允许开发人员控制错误消息的详细程度。当您部署一个应用程序时，这对于隐藏可能包含在跟踪中的敏感信息对用户来说非常有用。

**重要的错误修复**

- 修复了[错误](https://github.com/streamlit/streamlit/issues/1957)，其中 `st.image` 未正确渲染某些类型的 SVG。
- 修复了 [回归](https://github.com/streamlit/streamlit/issues/2699)，在鼠标悬停时仅显示 `st.slider` 的当前值。

## 版本 0.76.0

_发布日期：2021 年 2 月 4 日_

**重要更改**

- 🎨 [`st.color_picker`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.color_picker) 现在已经不再是 beta 版本。这意味着过去 3 个月被标记为弃用的旧的 beta_color_picker 函数现已被 color_picker 取代。
- 🐍 当直接运行 `python script.py` 时，显示一个警告。
- [`st.image`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.image) 的 `use_column_width` 现在默认为 `auto` 选项，如果图像超出列宽，它将自动调整图像大小以适应列宽。
- ✂️ 修复了在 [`st.beta_expander`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.beta_expander) 中内容被截断的 bug ([2437](https://github.com/streamlit/streamlit/issues/2437) 和 [2247](https://github.com/streamlit/streamlit/issues/2247))
- 📜 修复了 [`st.dataframe`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.dataframe) 中滚动条与最后一列内容重叠的 bug ([bug](https://github.com/streamlit/streamlit/issues/2543))
- 💾 修复了[`st.file_uploader`](https://docs.streamlit.io/en/0.76.0/api.html#streamlit.file_uploader)中的一个[bug](https://github.com/streamlit/streamlit/issues/2561)，其中返回的文件数据不是最近上传的文件。
- ➕ 修复了一些LaTeX命令未正确渲染的bug([2086](https://github.com/streamlit/streamlit/issues/2086)和[2556](https://github.com/streamlit/streamlit/issues/2556))。

## 版本0.75.0

发布日期：2021年1月21日

**重要变更**

- 🕳 [`st.empty`](https://docs.streamlit.io/en/0.75.0/api.html#streamlit.empty) 之前会在脚本结束时清除组件。现在已更新为立即清除组件。
- 🛹 之前在宽模式下，网页周围有较窄的边距。现在增加了边距，以提供更好的视觉体验。

## 版本 0.74.0

_发布日期: 2021 年 1 月 6 日_

**重要变更**

- 💾 [`st.file_uploader`](https://docs.streamlit.io/en/0.74.0/api.html#streamlit.file_uploader)已经稳定，并且已删除了弃用警告和相关的配置选项(`deprecation.showfileUploaderEncoding`)。
- 📊 在页面加载时，[`st.bokeh_chart`](https://docs.streamlit.io/en/0.74.0/api.html#streamlit.bokeh_chart)不再重复出现。
- 🎈 修复了页面图标，以支持带有变体（例如🤦‍♀️与🤦🏼‍♀️）或破折号（例如🌙-新月）的表情符号。

## 版本 0.73.0

_发布日期：2020年12月17日_

**重要变更**

- 🐍 Streamlit 现在可以在 Python 3.9 上安装。Streamlit 组件目前还不支持 Python 3.9，必须使用 3.8 或更早的版本。
- 🧱 Streamlit 组件现在支持同源策略，可以使用浏览器提供的功能，例如摄像头组件。
- 🐙 修复 Streamlit 分享部署体验，适用于使用 Git 版本 2.7.0 或更早版本的用户。
- 🧰 处理[`st.file_uploader`](https://docs.streamlit.io/en/0.72.0/api.html#streamlit.file_uploader)上传文件意外关闭的情况。

## 版本0.72.0

_发布日期：2020年12月2日_

**重要变更**

- 🌈 建立主题框架并迁移现有组件。
- 📱 改进移动设备上的侧边栏体验。
- 🧰 更新[`st.file_uploader`](https://docs.streamlit.io/en/0.71.0/api.html#streamlit.file_uploader)，以减少重新运行。

## 版本0.71.0

_发布日期：2020年11月11日_

**重要变更**

- 📁 更新 [`st.file_uploader`](https://docs.streamlit.io/en/0.71.0/api.html#streamlit.file_uploader) 以在应用程序重新运行时自动重置缓冲区。
- 📊 优化图表的默认渲染，并减少初始渲染的问题。

## 版本 0.70.0

_发布日期：2020年10月28日_

**重要变更**

- 🧪 [`st.set_page_config`](https://docs.streamlit.io/en/0.70.0/api.html#streamlit.set_page_config)和[`st.color_picker`](https://docs.streamlit.io/en/0.70.0/api.html#streamlit.color_picker)现已移入Streamlit命名空间。这些将于2021年1月28日从beta版本中移除。了解有关我们的beta过程的更多信息，请点击[这里](https://docs.streamlit.io/en/0.70.0/api.html#beta-and-experimental-features)。
- 📊 改进离散值柱状图的显示。

## 版本0.69.0

_发布日期：2020年10月15日_

**亮点：**

- 🎁 推出Streamlit sharing，这是部署、管理和共享公共Streamlit应用程序的最佳方式，完全免费。在我们的[博客文章](http://blog.streamlit.io/introducing-streamlit-sharing/)中了解更多信息，或者在[这里](https://streamlit.io/sharing)注册！
- 添加了`st.experimental_rerun`以通过编程方式重新运行您的应用程序。感谢[SimonBiggs](https://github.com/SimonBiggs)！

**重要更改**

- 📹 对于 st.video，增加了对于不同浏览器的开始和结束时间的更好支持。
- 🖼 修复了媒体文件偶尔失败的 bug。
- 📦 修复了自定义组件在 Safari 上的兼容性问题。请确保升级到最新的 [streamlit-component-lib](https://www.npmjs.com/package/streamlit-component-lib)。

## 版本 0.68.0

发布日期：2020年10月8日

**亮点：**

- ⌗ 引入了 Streamlit 的新布局选项！垂直布局，你退后吧。
  为水平布局腾出一些空间！查看我们的[博文](https://blog.streamlit.io/introducing-new-layout-options-for-streamlit/)。

- 💾 文件上传器重新设计，具有多文件上传的新功能，并更好地支持使用上传的文件。这可能会引起破坏性变化。请在我们的[文档](https://docs.streamlit.io/en/0.68.0/api.html#streamlit.file_uploader)中查看新的API。

**重要变更**

- 🎈 `st.balloon` 经过优化，现在拥有更漂亮的气球和更平滑的动画效果。
- 🚨 破坏性变更：在2020年1月废弃了 `st.deck_gl_chart`，现在我们完全移除了该API。请使用 `st.pydeck_chart` 替代。
- 🚨 破坏性变更：在2020年1月废弃了 `st.altair_chart`、`st.graphviz_chart`、`st.plotly_chart` 和 `st.vega_lite_chart` 的 `width` 和 `height` 参数，现在我们完全移除了这些参数。
  请在相应的图表库中设置宽度和高度。

## 版本0.67.0

发布日期：2020年9月16日

**亮点：**

- 🦷 Streamlit组件现在可以返回字节给您的Streamlit应用程序。要创建一个返回字节的组件，请确保升级到最新的[streamlit-component-lib](https://www.npmjs.com/package/streamlit-component-lib)。

**重要更改**

- 📈 弃用警告：从2020年12月1日起，`st.pyplot()`将需要一个图表作为参数。
  被提供。要禁用弃用警告，请将`deprecation.showPyplotGlobalUse`设置为`False`。
- 🎚 当处理大型数据集时，`st.multiselect`和`st.select`现在速度非常快。感谢[masa3141](https://github.com/masa3141)！

## 版本0.66.0

发布日期：2020年9月1日

**亮点：**

- ✏️ `st.write`现在可以在侧边栏中使用！
- 🎚 现在可以使用`st.select_slider`来选择不同或非数值的值。
- ⌗ Streamlit组件现在可以将数据帧返回到您的Streamlit应用程序中。请查看我们的[SelectableDataTable示例](https://github.com/streamlit/component-template/tree/master/examples/SelectableDataTable)。
- 📦 我们在Streamlit组件模板中使用的Streamlit组件库现在作为npm包（[streamlit-component-lib](https://www.npmjs.com/package/streamlit-component-lib)）提供，以简化将来对最新版本的升级。
  现有的组件无需迁移。

**重要变更**

- 🐼 支持从pandas版本1.0.0开始的StringDtype
- 🧦 支持在Unix套接字上运行Streamlit

## 版本0.65.0

发布日期：2020年8月12日

**亮点：**

- ⚙️ 通过st.beta_set_page_config()方法，可以设置页面标题、favicon、侧边栏状态和宽模式。详细信息请参阅我们的[文档](https://docs.streamlit.io/en/0.65.0/api.html#streamlit.set_page_config)。
- 📝 通过使用st.experimental_set_query_params和st.experimental_get_query_params，通过查询参数添加有状态的行为。感谢[@zhaoooyue](https://github.com/zhaoooyue)！
- 🐼 改进了对st.radio、st.selectbox和st.multiselect的pandas数据帧支持。
- 🛑 使用st.stop可以退出Streamlit应用程序。
- 🖼 对st.image添加了内联SVG支持。

**提示：**

- 🚨 弃用警告：st.image的参数格式已更名为output_format。

## 版本0.64.0

_发布日期: 2020年7月23日_

**亮点:**

- 📊 默认使用紧凑布局显示matplotlib图表。要禁用此功能，请将`bbox_inches`设置为`None`、字符串格式的英寸或`Bbox`对象。
- 🗃 在`st.file_uploader`上自动编码的功能已弃用，会发出警告。
- 🙈 如果`gatherUserStats`设置为`False`，则不会加载Segment库。感谢[@tanmaylaud](https://github.com/tanmaylaud)!

## 版本0.63.0

_发布日期: 2020年7月13日_

**亮点:**

- 🧩 **支持 Streamlit 组件！！！** 请查看[文档](https://docs.streamlit.io/en/latest/streamlit_components.html)获取更多信息。
- 🕗 在[`st.slider`](https://docs.streamlit.io/en/latest/api.html#streamlit.slider)中支持日期时间。当然，就像在`st.slider`中使用的任何其他值一样，您也可以传入含有两个元素的列表来获取一个日期时间范围滑块。

## 版本 0.62.0

_发布日期：2020年6月21日_

**亮点：**

- 📨 通过配置选项`server.enableWebsocketCompression`，可以打开/关闭WebSocket压缩功能。如果您的服务器会剥离HTTP头部并且无法更改此行为，这将非常有用。
- 🗝️ 使用[Cookie-to-header token](https://en.wikipedia.org/wiki/Cross-site_request_forgery#Cookie-to-header_token)技术，Streamlit提供了开箱即用的CSRF保护支持。这意味着如果您从多个副本中提供Streamlit应用程序，则需要
  配置它们以使用与`server.cookieSecret`配置选项相同的cookie密钥。
要关闭XSRF保护，请将`server.enableXsrfProtection=false`设置为true。

**重要的错误修复:**

- 🖼️ 添加了一个优雅期来修复与`st.image`或`st.pyplot`一起发送的图像有时丢失的多个相关错误的图像缓存过期逻辑。

## 版本0.61.0

_发布日期: 2020年6月2日_

**亮点:**

- 📅 支持在`st.date_picker`中使用日期范围。请参考
  [文档](https://docs.streamlit.io/en/latest/api.html#streamlit.date_picker) 获取更多信息，但简而言之，只需将列表/元组作为默认日期传递，它将被解释为范围。
- 🗣️ 现在您可以选择`st.echo`是将代码打印在输出之上还是之下。要了解更多信息，请参考[文档](https://docs.streamlit.io/en/latest/api.html#streamlit.echo)中的`code_location`参数。
- 📦 对Keras模型和Tensorflow `saved_models`的`@st.cache`支持进行了改进。

## 版本 0.60.0

_发布日期：2020 年 5 月 18 日_

**亮点：**

- ↕️ 可以使用`height`参数设置`st.text_area`的高度（以像素为单位）。详见
  [文档](https://docs.streamlit.io/en/latest/api.html#streamlit.text_area)。
- 🔡 可以设置在`st.text_area`或`st.text_input`中允许的最大字符数。请查看`max_chars`参数。
  [文档](https://docs.streamlit.io/en/latest/api.html#streamlit.text_area)。
- 🗺️ 对[H3](https://h3geo.org/)地理空间索引系统的更好的DeckGL支持。现在您可以在[`st.pydeck_chart`](https://docs.streamlit.io/en/latest/api.html#streamlit.pydeck_chart)中使用`H3HexagonLayer`等功能。
- 📦 改进的对PyTorch TensorBase和Model的`@st.cache`支持。

## 版本0.59.0

发布日期：2020年5月5日

**亮点：**

- 🎨 新的颜色选择器小部件！可以与其他小部件一起使用。
  [`st.beta_color_picker()`](https://docs.streamlit.io/en/0.69.0/api.html#streamlit.beta_color_picker)
- 🧪 引入`st.beta_*`和`st.experimental_*`函数前缀，以加快Streamlit功能的发布速度。更多信息请参见[文档](https://docs.streamlit.io/en/latest/api.html#pre-release-features)。
- 📦 改进了对SQL Alchemy对象、CompiledFFI、PyTorch张量和`builtins.mappingproxy`的`@st.cache`支持。

## 版本0.58.0

发布日期：2020年4月22日

**亮点：**

- 💼 使 `st.selectbox` 的过滤不区分大小写。
- ㈬ 更好地支持 `@st.cache` 中的Tensorflow会话。
- 📊 更改 `st.pyplot` 的行为，仅在使用全局Matplotlib图形时自动清除图形（即仅在调用 `st.pyplot()` 而不是 `st.pyplot(fig)` 时）。

## 版本 0.57.0

_发布日期：2020年3月26日_

**亮点：**

- ⏲️ 通过设置 `max_entries` 和 `ttl` 参数，为 `@st.cache` 缓存函数设置过期选项。
  详见
  [文档](https://docs.streamlit.io/en/latest/api.html#streamlit.cache)。
- 🆙 改进了`st.file_uploader`的内部机制，现在性能更好！还增加了默认上传限制为200MB（可通过`server.max_upload_size`进行配置）。
- 🔒 `server.address`配置选项现在将服务器绑定到该地址以增加安全性。
- 📄 为`@st.cache`添加了更多详细信息，以便更容易进行调试。

## 版本0.56.0

发布日期：2020年2月15日

**亮点:**

- 📄 st.cache的错误消息得到改进。错误消息现在也指向了我们刚刚发布的新缓存文档。在此处阅读更多信息：[here](https://discuss.streamlit.io/t/help-us-stress-test-streamlit-s-latest-caching-update/1944)！

**重大变更:**

- 🐍 如[上个月宣布的](https://discuss.streamlit.io/t/streamlit-will-deprecate-python-2-in-february/1656)，**Streamlit不再支持Python 2**。要使用Streamlit，您需要Python 3.5或更高版本。

## 版本 0.55.0

_发布日期：2020年2月4日_

**亮点：**

- 📺 **能够直接从Streamlit录制屏幕录像！** 这样您就可以轻松记录和分享有关您的模型、分析、数据等的解释。只需点击 ☰ 然后选择"Record a screencast"。试试看吧！

## 版本0.54.0

_发布日期：2020年1月29日_

**亮点：**

- ⌨️ 支持密码字段！只需将`type="password"`传递给`st.text_input()`。

**值得注意的修复：**

- ✳️ st.cache改进，包括对复杂对象的更好支持。
- 🗣️ 修复了多个用户之间侧边栏的交叉对话问题。

**重大变更：**

- 如果您正在使用SessionState的“hack” Gist，请重新下载！
  根据您使用的哪种hack，以下是一些链接，可帮助您节省一些时间：
  - [SessionState.py](https://gist.github.com/tvst/036da038ab3e999a64497f42de966a92)
  - [st_state_patch.py](https://gist.github.com/tvst/0899a5cdc9f0467f7622750896e6bd7f)

## 版本0.53.0

发布日期：2020年1月14日

**亮点：**

- 🗺️ 支持所有DeckGL功能！只需使用[Pydeck](https://deckgl.readthedocs.io/en/latest/)替代[`st.deck_gl_chart`](https://docs.streamlit.io/en/latest/api.html#streamlit.pydeck_chart)即可。要做到这一点，只需将PyDeck对象传递给[`st.pydeck_chart`](https://docs.streamlit.io/en/latest/api.html#streamlit.pydeck_chart)。
  [`st.write`](https://docs.streamlit.io/en/latest/api.html#streamlit.write)或[magic](https://docs.streamlit.io/en/latest/api.html#magic)。

请注意，由于这是一个**预览版本**，在不久的将来可能会发生变化。在我们稳定API之前，期待听到社区的意见！

**目标是取代`st.deck_gl_chart`**，因为它可以做到旧API的所有功能，而且功能更强大！

- 🆕 在开发过程中对Streamlit升级的处理更加优化。现在，如果显示的应用程序使用的是比标签页运行的Streamlit版本更新的版本，我们会自动重新加载浏览器标签页。

- 👑 新的网站图标，带有我们的新标识！

**主要修复:**

- 在Python 3.8中，Magic现在可以正常工作。它不再导致应用程序中的文档字符串被渲染。

**重要更改:**

- 更新了如何计算所有图表类型的默认宽度和高度的方式。
  我们现在将图表大小的设置交给图表库本身处理，请参阅图表库的文档。

因此，大多数图表命令中的`width`和`height`参数已被弃用，并且引入了`use_container_width`参数，以使图表占据尽可能多的水平空间（这曾是默认设置）。

## 版本 0.52.0

_发布日期：2019年12月20日_

**亮点：**

- 📤 文件上传小部件的预览版本。只需调用[`st.file_uploader`](https://docs.streamlit.io/en/latest/api.html#streamlit.file_uploader)尝试它！

  _请注意，作为**预览版本**，在不久的将来可能会发生变化。在我们稳定API之前，期待社区的反馈！_

- 👋 在`st.write`和`st.markdown`中支持[表情符号代码](https://www.webfx.com/tools/emoji-cheat-sheet/)！使用`st.write("Hello :wave:")`试试吧。

**重大变更：**

- 🧹 `st.pyplot` 现在默认清除图形，因为在大多数情况下这是您想要的。这样您就可以在不必每次调用[`pyplot.clf`](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.clf.html)的情况下创建两个或更多的Matplotlib图表。如果您想关闭此行为，请使用[`st.pyplot(clear_figure=False)`](https://docs.streamlit.io/en/latest/api.html#streamlit.pyplot)
- 📣 `st.cache` 不再检查输入的变化。这是我们简化缓存系统并为 Streamlit 准备其他缓存原语（如 Session State）启动的首个改变！

## 版本 0.51.0

_发布日期：2019 年 11 月 30 日_

**亮点：**

- 🐕 现在您可以通过配置选项 `server.fileWatcherType` 来调整文件监视器的行为。使用它可以在以下选项之间切换：
  - `auto`（默认）：Streamlit 将尝试使用 watchdog 模块，
    如果 watchdog 不可用，则回退到轮询模式。
- `watchdog`：强制 Streamlit 使用 watchdog 模块。
- `poll`：强制 Streamlit 始终使用轮询。
- `none`：Streamlit 将不会监视文件。

**值得注意的错误修复：**

- 修复了静态报告共享中的 "keyPrefix" 选项 [#724](https://github.com/streamlit/streamlit/pull/724)
- 添加了对 DeckGL 图表的 getColorX 和 getTargetColorX 的支持 [#718](https://github.com/streamlit/streamlit/pull/718)
- 修复Windows上的Tornado和Python 3.8问题 [#682](https://github.com/streamlit/streamlit/pull/682)
- 如果Linux上未安装xdg-open，则回退到webbrowser [#701](https://github.com/streamlit/streamlit/pull/701)
- 修复Firefox上数字输入旋转按钮问题 [#683](https://github.com/streamlit/streamlit/pull/683)
- 修复Windows上的CTRL+ENTER问题 [#699](https://github.com/streamlit/streamlit/pull/699)
- 在无头模式下不会自动创建凭据文件 [#467](https://github.com/streamlit/streamlit/pull/467)

## 版本 0.50.1

_发布日期：2019 年 11 月 10 日_

**亮点：**

- 👩‍🎓 支持 SymPy，并能够使用 LaTeX 绘制数学表达式！请参见 [`st.latex`](/library/api-reference/text/st.latex)、[`st.markdown`](/library/api-reference/text/st.markdown) 和 [`st.write`](/library/api-reference/write-magic/st.write)。
- 🌄 现在可以使用环境变量来设置配置选项。例如，`export STREAMLIT_SERVER_PORT=9876`。
- 🐱 可以直接使用Github和Gist的URL来调用`streamlit run`。不需要先获取"raw" URL。
- 📃 更清晰的异常堆栈跟踪。我们现在从用户应用程序产生的堆栈跟踪中删除所有Streamlit特定的代码。

## 版本0.49.0

_发布日期：2019年10月23日_

**亮点：**

- 💯 新的输入小部件，可以使用键盘输入数字：`st.number_input()`
- 📺 音频/视频改进：能够从URL加载，嵌入YouTube视频，并设置起始位置。
- 🤝 您现在可以将应用程序的静态快照分享到S3！请参阅`streamlit config show`中的S3部分进行设置。然后从右上角菜单进行分享。
- ⚙️ 使用`server.baseUrlPath`配置选项将Streamlit的URL设置为类似`http://domain.com/customPath`的内容。

**重要的错误修复：**

- 修复了许多Windows上的错误，包括[问题#339](https://github.com/streamlit/streamlit/issues/399)和[#401](https://github.com/streamlit/streamlit/issues/301)。

## 版本0.48.0

_发布日期：2019年10月12日_

**亮点：**

- 🔧 现在可以通过命令行标志或本地配置文件设置配置选项。
- ↕️ 现在可以最大化图表和图片！
- ⚡ 当快速连续写入数据到应用程序时，Streamlit现在速度大大提升。
- ✳️ 支持从“保存时运行”和`@st.cache`散列中屏蔽文件夹通配符。
- 🎛️ 在修改Python文件时，改进小部件状态的处理方式。
- 🙈 在`st.write`和`st.markdown`中改进了对HTML的支持。但是HTML仍然不安全！

**值得注意的错误修复:**

- 修复了与将Python环境设置为当前工作目录相关的`@st.cache`错误。[问题＃242](https://github.com/streamlit/streamlit/issues/242)
- 修复了在Windows上加载根URL `/`的问题。[Issue #244](https://github.com/streamlit/streamlit/issues/244)

## 版本0.47.0

_发布日期：2019年10月1日_

**亮点：**

- 🌄 新增hello.py示例，展示了4个精彩的Streamlit应用程序。快来试试吧！
- 🔄 当8501端口已被占用时，Streamlit现在会自动选择一个未使用的端口。
- 🎁 侧边栏支持现已正式发布！只需以`st.sidebar.`开头，而不是`st.`即可使用。
- ⚡ 性能改进：我们在websocket层添加了缓存，因此当数据在运行之间没有变化时，我们不再重新发送数据到浏览器
- 📈 我们的“原生”图表`st.line_chart`、`st.area_chart`和`st.bar_chart`现在在幕后使用Altair
- 🔫 改进的小部件：自定义的st.slider标签；多选中的默认值
- 🕵️‍♀️ 文件系统监视器现在忽略隐藏的文件夹和虚拟环境
- 💅 在缓存和小部件状态管理方面有很多细节优化

**重大变更：**

- 🛡️我们暂时停用了对Streamlit应用程序的静态“快照”共享的支持。现在我们不再是一个有限制的测试版，我们需要确保共享功能经过深思熟虑，并遵守DMCA等法律。但我们正在努力解决这个问题！

## 版本0.46.0

发布日期：2019年9月19日

**亮点：**

- ✨魔术命令！无需输入`st.write`，即可使用`st.write`。请参阅
  <https://docs.streamlit.io/en/latest/api.html#magic-commands>
- 🎛️ 新增 `st.multiselect` 小部件。
- 🐍 修复了许多安装问题，现在您可以使用 `pip install streamlit`，即使在Conda中也可以使用！我们已经停用了我们的Conda仓库。
- 🐞 多个错误修复和额外的优化，为我们的发布做准备！

**重大变更：**

- 🛡️ 默认情况下，在 `st.write`/`st.markdown` 中禁用 HTML 标签。更多信息和临时解决方法请参考：<https://github.com/streamlit/streamlit/issues/152>

## 版本 0.45.0

_发布日期: 2019年8月28日_

**亮点:**

- 😱 实验性支持_侧边栏_！如果您想成为测试人员，请告诉我们。
- 🎁 完全重新设计的`st.cache`！性能更强大，API更简洁，支持缓存被`@st.cached`函数调用的函数，用户友好的错误信息等等！
- 🖼️ `st.image`速度极快，能够选择JPEG和PNG压缩方式，以及RGB和BGR（用于OpenCV）之间的选择。
- 💡 对`st.slider`、`st.selectbox`和`st.radio`提供更智能的API。
- 🤖 自动修复Matplotlib后端，无需编辑.matplotlibrc文件。

## 版本0.44.0

_发布日期：2019年7月28日_

**亮点：**

- ⚡ 当您在Streamlit代码上执行ctrl-c/rerun时，重新连接速度非常快
- 📣 在连接失败时提供有用的错误信息
- 💎 修复了多个错误并改进了我们新发布的交互式小部件的功能和优化

## 版本0.43.0

_发布日期：2019年7月9日_

**亮点：**

- ⚡ 支持交互式小部件！🎈🎉

## 版本 0.42.0

发布日期：2019年7月1日

**亮点：**

- 💾 支持将Vega-Lite和Altair图表保存为SVG或PNG格式
- 🐇 现在在浏览器中缓存JS文件，以加快加载速度
- ⛔ 改进了Streamlit应用程序内的错误处理

## 版本 0.41.0

发布日期：2019年6月24日

**亮点：**

- 📈 大大改进了对Vega-Lite和Altair中命名数据集的支持
- 🙄 增加了在监视文件更改时忽略某些文件夹的功能。请参阅`server.folderWatchBlacklist`配置选项。
- ☔ 在用户脚本和导入模块的语法错误方面更加健壮。

## 版本0.40.0

_发布日期：2019年6月10日_

**亮点：**

- Streamlit的速度提高了10倍以上。只需保存并观察您的分析即可立即更新。
- 我们更改了如何运行Streamlit应用程序：
  `$ streamlit run your_script.py [script args]`
- 与之前版本的Streamlit不同，`streamlit run [script] [script args]`会创建一个服务器（现在您不需要担心代理是否正常工作）。要关闭服务器，您只需按下**Ctrl+c**。

**为什么速度更快？**

现在，Streamlit会在关闭服务器之前保持单个Python会话运行。这意味着Streamlit可以重新运行您的代码，而无需启动新的进程；导入的库被缓存到内存中。一个额外的好处是`st.cache`现在将缓存到内存而不是磁盘。

**如果我以旧的方式运行Streamlit会发生什么？**

如果您运行`$ python your_script.py`，脚本将从上到下执行，但不会生成一个Streamlit应用程序。

**新架构的限制是什么？**

- 要切换Streamlit应用程序，首先需要使用**Ctrl-c**停止Streamlit服务器。然后，您可以使用`streamlit run`命令生成下一个应用程序。
- Streamlit只能在Python文件中使用，不能在Python REPL中进行交互使用。

**还有什么需要知道的？**

- 当**liveSave**开启时，我们在命令行打印的字符串已经被清理过了。您可能需要调整任何依赖于这些字符串的正则表达式。
- 一些配置选项已经被重命名：

  | 旧配置                      | 新配置                  |
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

如果新的Streamlit无法正常工作，请通过Slack或电子邮件与我们联系。您可以随时使用以下命令降级：

```bash
pip install --upgrade streamlit==0.37
```

```bash
conda install streamlit=0.37
```

**接下来是什么？**

感谢您与我们一起走过这段旅程！这个版本的Streamlit为交互式小部件打下了基础，这是Streamlit的一个全新功能，我们非常期待在接下来的几个月与您分享。

## 版本0.36.0

_发布日期：2019年5月3日_

**亮点**

- 🚣‍♀️ `st.progress()` 现在也接受从0.0到1.0的浮点数
- 🤯 改进了数据框中长标题的渲染效果
- 🔐 共享应用默认使用HTTPS

## 版本0.35.0

_发布日期：2019年4月26日_

**亮点**

- 📷 支持Bokeh！请查看文档中的 `st.bokeh_chart`
- ⚡️ 改进了保存应用程序的大小和加载时间
- ⚾️ 在整个代码库中实现了更好的错误捕获机制
