---
title: 如何让Streamlit监视我在应用程序中导入的其他模块的更改？
slug: /knowledge-base/using-streamlit/streamlit-watch-changes-other-modules-importing-app
---

# 如何让Streamlit监视我在应用程序中导入的其他模块的更改？

默认情况下，Streamlit只监视主应用程序模块所在目录中的模块。您可以通过将每个模块的父目录添加到`PYTHONPATH`来跟踪其他模块的更改。

```bash
export PYTHONPATH=$PYTHONPATH:/path/to/module
streamlit run your_script.py
```
