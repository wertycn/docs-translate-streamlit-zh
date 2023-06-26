---
title: Magic
slug: /library/api-reference/write-magic/magic
---

## 魔法命令

魔法命令是Streamlit中的一项功能，允许您在不需要键入显式命令的情况下编写几乎任何内容（markdown、数据、图表）。只需将您想要显示的内容放在自己的代码行上，它将出现在您的应用程序中。以下是一个示例：

```python
# Draw a title and some text to the app:
'''
# This is the document title

This is some _markdown_.
'''

import pandas as pd
df = pd.DataFrame({'col1': [1,2,3]})
df  # 👈 Draw the dataframe

x = 10
'x', x  # 👈 Draw the string 'x' and then the value of x

# Also works with most supported chart types
import matplotlib.pyplot as plt
import numpy as np

arr = np.random.normal(1, 1, size=100)
fig, ax = plt.subplots()
ax.hist(arr, bins=20)

fig  # 👈 Draw a Matplotlib chart
```

### Magic的工作原理

每当Streamlit在单独一行上看到变量或文字值时，它会自动使用[`st.write`](/library/api-reference/write-magic/st.write)将其写入您的应用程序（您稍后会了解更多信息）。

此外，Magic足够智能，可以忽略文档字符串。也就是说，它会忽略文件和函数顶部的字符串。

如果您更喜欢显式调用Streamlit命令，您始终可以进行调整。
在您的 `~/.streamlit/config.toml` 文件中，关闭魔法模式，并使用以下设置:

```toml
[runner]
magicEnabled = false
```

<重要>
<p>目前，Magic只能在主要的Python应用程序文件中工作，而不能在导入的文件中工作。请参阅GitHub问题＃288，了解相关问题的讨论。</p>
</重要>

### 特色视频

了解[`st.write`](/library/api-reference/write-magic/st.write)和[magic](/library/api-reference/write-magic/magic)命令是什么以及如何使用它们。

<YouTube videoId="wpDuY9I2fDg" />
