---
title: 修改图表
slug: /library/api-reference/mutate
description: st.add_rows可以在某些元素中将一个数据框追加到当前数据框的底部，以进行优化的数据更新。
---

# 修改图表

有时候你需要显示一个图表或数据框，并且希望在应用程序运行时实时修改（例如，在循环中）。根据你的需求，有三种不同的方法可以实现这一点：

1. 使用[`st.empty`](/library/api-reference/layout/st.empty)来替换单个元素。
2. 使用[`st.container`](/library/api-reference/layout/st.container)或[`st.columns`](/library/api-reference/layout/st.columns)来替代多个元素。
3. 使用`add_rows`将数据追加到特定类型的元素中。

这里我们讨论的是最后一种情况。

<Autofunction function="DeltaGenerator.add_rows" />
