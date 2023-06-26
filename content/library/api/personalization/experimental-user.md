---
title: st.experimental_user
slug: /library/api-reference/personalization/st.experimental_user
description: st.experimental_user返回有关Streamlit Community Cloud上私有应用程序的已登录用户的信息。

# st.experimental_user

<Important>

这是一个实验性功能。实验性功能及其API可能随时更改或移除。了解更多信息，请点击[这里](/library/advanced-features/prerelease#experimental-features)。

</Important>

`st.experimental_user` 是 Streamlit 的一个命令，用于返回有关 Streamlit 社区云中已登录用户的信息。它允许开发者为查看应用程序的用户进行个性化设置。在私有的 Streamlit 社区云应用程序中，它返回一个包含查看者邮箱的字典。在公共的 Streamlit 社区云应用程序中，该字段的值为空，以防止将用户邮箱泄露给开发者。

API的设计与[`st.session_state`](/library/api-reference/session-state)和[`st.secrets`](/streamlit-community-cloud/get-started/deploy-an-app/connect-to-data-sources/secrets-management)非常相似，采用了基于字段的API，与Python字典非常相似。

### 允许的字段

`st.experimental_user`命令返回一个只有一个字段`email`的字典。

通过将该命令传递给`st.write`，显示允许的字段。

```python
# Display the contents of the dictionary
st.write(st.experimental_user)
```

上面显示了一个包含一个字段和值的字典。该字段始终为`email`：

```json
{
  "email": "value"
}
```

您可以检查 `st.experimental_user` 中是否存在一个字段：

```python
# Returns True if the field exists
"email" in st.experimental_user

# Returns False if the field does not exist
"name" in st.experimental_user
```

### 读取值

通过传递给 `st.write`，读取 `email` 字段的值并显示出来：

```python
# Dictionary like API
st.write(st.experimental_user['email'])

# Attribute API
st.write(st.experimental_user.email)
```

上面的输出可能是`None`，也可能是已登录用户的电子邮箱或者`test@localhost.com`，具体取决于应用程序运行的位置。请继续阅读了解`st.experimental_user`的上下文相关行为。

### 更新和修改

`st.experimental_user`的键和值**不能**被更新或修改。如果您尝试更新它们，Streamlit会抛出一个方便的`StreamlitAPIException`异常:

```python
st.experimental_user.name = None
# Throws an exception!

st.experimental_user.email = "hello"
# Throws an exception!
```

<img src="/images/st-user-value-exception.png" />

### 上下文相关的行为

`st.experimental_user.email` 的值是上下文相关的。它根据应用程序运行的位置返回一个值。[私有](#private-app-on-streamlit-cloud) 或 [公共](#public-app-on-streamlit-cloud) 应用程序可以在 Streamlit Community Cloud 上运行，[本地](#local-development)，或者在[第三方云提供商](#app-deployed-on-a-3rd-party-cloud-provider)上。让我们来看看不同的场景。

#### **Streamlit社区云上的私有应用**

用户需要登录Streamlit社区云才能查看私有应用。如果用户未登录，他们将看到：

<div style={{ maxWidth: '65%', marginBottom: '-1em', marginLeft: '8em' }}>
    <Image src="/images/private-app-access.png" />
</div>

如果用户已登录，`st.experimental_user.email` 将返回用户的电子邮件地址。假设一个用户使用jane@email.com登录了Streamlit社区云：

```python
st.experimental_user.email
# Returns: jane@email.com
```

#### **在Streamlit Community Cloud上的公共应用程序**

目前，`st.experimental_user.email`可以返回Streamlit Community Cloud上私有应用程序中已登录用户的信息。如果在公共应用程序中使用，它将返回`None`。例如：

```python
st.experimental_user.email
# Returns: None
```

这个字段在公共的Streamlit社区云应用中是空的，以防止将用户的电子邮件泄露给开发者。

#### **本地开发**

在本地开发时，`st.experimental_user.email`返回的是test@localhost.com。我们不返回`None`，以便更容易地在本地测试该功能。例如：

```python
st.experimental_user.email
# Returns: test@localhost.com
```

#### **部署在第三方云服务提供商上的应用程序**

在将应用程序部署在第三方云服务提供商（例如Amazon EC2，Heroku等）上时，`st.experimental_user.email`的行为与本地开发时相同。例如：

```python
st.experimental_user.email # On a 3rd party cloud provider
# Returns: test@localhost.com
```

### 示例

为用户定制应用程序是使您的应用程序更具吸引力的一种很好的方式。

它为开发人员提供了许多用例，其中一些可能包括：为管理员显示附加控件，可视化用户的Streamlit历史记录，个性化的股票行情，聊天机器人应用程序等等。我们很期待看到您如何利用这个功能构建应用程序！

以下是一个代码片段，显示了管理员的额外按钮：

```python
# Show extra buttons for admin users.
ADMIN_USERS = {
    'person1@email.com',
    'person2@email.com',
    'person3@email.com'
}
if st.experimental_user.email in ADMIN_USERS:
    display_the_extra_admin_buttons()
display_the_interface_everyone_sees()
```

根据用户的电子邮件地址向其展示不同的内容：

```python
# Show different content based on the user's email address.
if st.experimental_user.email == 'jane@email.com':
    display_jane_content()
elif st.experimental_user.email == 'adam@foocorp.io':
    display_adam_content()
else:
    st.write("Please contact us to get access!")
```

使用存储在数据库中的用户名称来向用户打招呼:

```python
# Greet the user by their name.
if st.experimental_user.email:
    # Get the user's name from the database.
    name = get_name_from_db(st.experimental_user.email)
    st.write('Hello, %s!' % name)
```

### 注意事项和限制

- `st.experimental_user` 是**只读**的。您不能更新或修改它的值。这样做将抛出一个 `StreamlitAPIException` 异常。
- 仅当用户登录到 Streamlit Community Cloud 并且应用程序是私有的时，才会返回有效的电子邮件。否则，将返回 `None` 或 test@localhost.com。
- 这是一个实验性功能。实验性功能及其API可能随时更改或删除。了解更多信息，请点击[这里](/library/advanced-features/prerelease#experimental-features)。
