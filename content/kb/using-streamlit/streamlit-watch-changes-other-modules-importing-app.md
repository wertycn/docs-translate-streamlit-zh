---
slug: /knowledge-base/using-streamlit/streamlit-watch-changes-other-modules-importing-app
title: How can I make Streamlit watch for changes in other modules I'm importing in
  my app?
---

# 如何让Streamlit监听我在应用程序中导入的其他模块的更改？

默认情况下，Streamlit只会监听主应用模块所在目录中的模块。您可以通过将每个模块的父目录添加到`PYTHONPATH`来追踪其他模块的更改。

```bash
export PYTHONPATH=$PYTHONPATH:/path/to/module
streamlit run your_script.py
```