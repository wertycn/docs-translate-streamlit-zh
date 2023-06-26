---
description: st.echo displays some code on the app, then execute it. Useful for tutorials.
slug: /library/api-reference/utilities/st.echo
title: st.echo
---

<Autofunction function="streamlit.echo" />

### 显示代码

有时候您希望您的Streamlit应用程序既包含您通常的Streamlit图形元素，又包含生成这些元素的代码。这就是`st.echo()`的作用。

假设您有以下文件，并且希望通过在Streamlit应用程序中显示中间部分，使其更加自解释：

```python
import streamlit as st

def get_user_name():
    return 'John'

# ------------------------------------------------
# Want people to see this part of the code...

def get_punctuation():
    return '!!!'

greeting = "Hi there, "
user_name = get_user_name()
punctuation = get_punctuation()

st.write(greeting, user_name, punctuation)

# ...up to here
# ------------------------------------------------

foo = 'bar'
st.write('Done!')
```

上面的文件创建了一个包含词语"Hi there, `John`"和"Done!"的Streamlit应用程序。

现在让我们使用`st.echo()`来使代码中间部分在应用程序中可见：

```python
import streamlit as st

def get_user_name():
    return 'John'

with st.echo():
    # Everything inside this block will be both printed to the screen
    # and executed.

    def get_punctuation():
        return '!!!'

    greeting = "Hi there, "
    value = get_user_name()
    punctuation = get_punctuation()

    st.write(greeting, value, punctuation)

# And now we're back to _not_ printing to the screen
foo = 'bar'
st.write('Done!')
```

就是这么简单！

<注意>

您可以在同一个文件中拥有多个`st.echo()`块。
尽情使用吧！

</注意>