---
slug: /library/advanced-features/timezone-handling
title: Working with timezones
---

# 处理时区

通常情况下，处理时区可能会很棘手。您的Streamlit应用程序用户的时区不一定与运行应用程序的服务器相同。对于公共应用程序尤其如此，世界上任何地方（任何时区）的任何人都可以访问您的应用程序。因此，了解Streamlit如何处理时区非常重要，这样您就可以避免在显示`datetime`信息时出现意外行为。

## Streamlit如何处理时区

Streamlit始终在前端显示与后端中的相应`datetime`实例相同的`datetime`信息。即，日期或时间信息不会自动调整为用户的时区。我们区分以下两种情况：

### **没有时区的`datetime`实例（naive）**

当您提供一个没有指定时区的 `datetime` 实例时，前端会显示没有时区信息的 `datetime` 实例。例如（这也适用于其他小部件，如 [`st.dataframe`](/library/api-reference/data/st.dataframe)）：

```python
import streamlit as st
from datetime import datetime

st.write(datetime(2020, 1, 10, 10, 30))
# Outputs: 2020-01-10 10:30:00
```

以上应用程序的用户始终将输出显示为`2020-01-10 10:30:00`。

### **带时区的`datetime`实例**

当您提供一个带有时区的`datetime`实例时，前端将在相同的时区显示`datetime`实例。例如（这也适用于其他小部件，如[`st.dataframe`](/library/api-reference/data/st.dataframe)）：

```python
import streamlit as st
from datetime import datetime
import pytz

st.write(datetime(2020, 1, 10, 10, 30, tzinfo=pytz.timezone("EST")))
# Outputs: 2020-01-10 10:30:00-05:00
```

以上应用程序的用户总是将输出显示为`2020-01-10 10:30:00-05:00`。

在这两种情况下，日期和时间信息都不会自动调整为前端用户的时区。用户看到的与后端中相应的`datetime`实例是完全相同的。当前无法自动调整日期或时间信息以适应查看应用程序的用户的时区。

<注意>

`st.dataframe`的旧版本存在时区问题。我们不计划为旧的数据框架提供额外的修复或增强功能。如果您需要稳定的时区支持，请考虑通过更改[配置设置](/library/advanced-features/configuration#set-configuration-options)，将_config.dataFrameSerialization设置为"arrow"，切换到arrow序列化。

</Note>