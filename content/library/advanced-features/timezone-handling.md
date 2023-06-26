---
title: 处理时区
slug: /library/advanced-features/timezone-handling
---

# 处理时区

一般来说，处理时区可能会比较棘手。您的Streamlit应用程序用户的时区不一定与运行应用程序的服务器相同。尤其是对于公共应用程序来说，世界上任何时区的任何人都可以访问您的应用程序。因此，了解Streamlit如何处理时区是非常重要的，这样您就可以避免在显示`datetime`信息时出现意外行为。

## Streamlit如何处理时区

Streamlit在前端始终显示与后端相应的`datetime`实例相同的`datetime`信息。也就是说，日期或时间信息不会自动调整为用户的时区。我们区分以下两种情况：

### **没有时区的`datetime`实例（naive）**

当您提供一个没有指定时区的`datetime`实例时，前端将显示没有时区信息的`datetime`实例。例如（这也适用于其他小部件，如[`st.dataframe`](/library/api-reference/data/st.dataframe)）：

```python
import streamlit as st
from datetime import datetime

st.write(datetime(2020, 1, 10, 10, 30))
# Outputs: 2020-01-10 10:30:00
```

上述应用的用户始终将输出显示为`2020-01-10 10:30:00`。

### **带有时区的`datetime`实例**

当您提供一个带有时区的`datetime`实例时，前端会在相同的时区显示该`datetime`实例。例如（这也适用于其他小部件，如[`st.dataframe`](/library/api-reference/data/st.dataframe)）：

```python
import streamlit as st
from datetime import datetime
import pytz

st.write(datetime(2020, 1, 10, 10, 30, tzinfo=pytz.timezone("EST")))
# Outputs: 2020-01-10 10:30:00-05:00
```

以上应用的用户始终在前端看到的输出为`2020-01-10 10:30:00-05:00`。

在这两种情况下，日期和时间信息都不会自动调整到用户的时区。用户看到的与后端中相应的`datetime`实例是相同的。目前无法自动调整日期或时间信息以适应查看应用程序的用户的时区。

<Note>

`st.dataframe`的旧版本存在时区问题。我们不打算为旧数据框推出更多的修复或增强功能。如果您需要稳定的时区支持，请考虑通过更改[配置设置](/library/advanced-features/configuration#set-configuration-options)，将_config.dataFrameSerialization设置为"arrow"，切换到arrow序列化。

</Note>
