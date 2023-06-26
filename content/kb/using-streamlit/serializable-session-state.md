---
title: 什么是可序列化的会话状态？
slug: /knowledge-base/using-streamlit/serializable-session-state
---

# 什么是可序列化的会话状态？

## 可序列化的会话状态

序列化是指将对象或数据结构转换为可以持久化和共享的格式，并允许您恢复数据的原始结构的过程。Python内置的[pickle](https://docs.python.org/3/library/pickle.html)模块将Python对象序列化为字节流（"pickling"），并将流反序列化为对象（"unpickling"）。

默认情况下，Streamlit的[会话状态](/library/advanced-features/session-state)允许您在会话期间持久化任何Python对象，无论对象是否可pickle序列化。这个属性使您可以存储Python的原始数据类型，如整数、浮点数、复数和布尔值，数据帧，甚至函数返回的[lambdas](https://docs.python.org/3/reference/expressions.html#lambda)。然而，一些执行环境可能要求在会话状态中序列化所有数据，因此在开发过程中或者当执行环境未来将不再支持时，检测不兼容性可能会很有用。

为此，Streamlit提供了一个`runner.enforceSerializableSessionState` [配置选项](https://docs.streamlit.io/library/advanced-features/configuration)，当设置为`true`时，只允许在会话状态中使用可pickle序列化的对象。要启用此选项，可以创建一个全局或项目配置文件，并使用以下内容，或者将其作为命令行标志使用：

```toml
# .streamlit/config.toml
[runner]
enforceSerializableSessionState = true
```

通过"_pickle-serializable_"，我们指的是调用`pickle.dumps(obj)`不应该引发[`PicklingError`](https://docs.python.org/3/library/pickle.html#pickle.PicklingError)异常。当启用配置选项时，向会话状态添加不可序列化的数据应该导致异常。例如，

```python
import streamlit as st

def unserializable_data():
		return lambda x: x

#👇 results in an exception when enforceSerializableSessionState is on
st.session_state.unserializable = unserializable_data()
```

![UnserializableSessionStateError](/images/unserializable-session-state-error.png)
