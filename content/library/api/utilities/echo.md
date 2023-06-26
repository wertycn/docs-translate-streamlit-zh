---
标题：st.echo
网址：/library/api-reference/utilities/st.echo
描述：st.echo在应用程序上显示一些代码，然后执行它。对于教程非常有用。

<Autofunction function="streamlit.echo" />

### 显示代码

有时候，您希望您的Streamlit应用程序既包含常规的Streamlit图形元素，又包含生成这些元素的代码。这就是`st.echo()`的用处所在。

好的，假设您有以下文件，并且希望将其
通过使Streamlit应用程序中的中间部分可见，使应用程序更加自解释。

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

现在让我们使用`st.echo()`来使代码的中间部分在应用程序中可见：

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

这很简单！

<注意>

您可以在同一个文件中使用多个`st.echo()`块。
随意使用它！

</注意>
