---
description: st.add_rows appends a dataframe to the bottom of the current one in certain
  elements, for optimized data updates.
slug: /library/api-reference/mutate
title: Mutate charts
---

# 修改图表

有时您会显示一个图表或数据框，并希望在应用程序运行时实时修改（例如，在循环中）。根据您的需求，有三种不同的方法可以实现这一点：

1. 使用 [`st.empty`](/library/api-reference/layout/st.empty) 来替换单个元素。
2. 使用 [`st.container`](/library/api-reference/layout/st.container) 或 [`st.columns`](/library/api-reference/layout/st.columns) 来替换多个元素。
3. 使用 `add_rows` 方法将数据附加到特定类型的元素。

这里我们讨论最后一种情况。

<Autofunction function="DeltaGenerator.add_rows" />