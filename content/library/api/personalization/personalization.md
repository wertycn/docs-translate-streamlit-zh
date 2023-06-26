---
title: 个性化用户应用程序
slug: /library/api-reference/personalization
---

# 个性化用户应用程序

<TileContainer>
<RefCard href="/library/api-reference/personalization/st.experimental_user" size="two-third">

#### 用户信息

`st.experimental_user` 返回有关 Streamlit 社区云上私有应用程序中已登录用户的信息。

```python
if st.experimental_user.email == "foo@corp.com":
  st.write("Welcome back, ", st.experimental_user.email)
else:
  st.write("You are not authorized to view this page.")
```

</RefCard>
</TileContainer>
