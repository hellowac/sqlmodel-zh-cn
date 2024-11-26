# 使用 FastAPI 更新附加数据（哈希密码）

在上一章中，我向你解释了如何从 **FastAPI** *路径操作* 接收到的输入数据更新数据库中的数据。

现在，我将向你解释如何在更新或创建模型对象时，添加 **附加数据**，即除了输入数据之外的数据。

当你需要在代码中 **生成一些数据**，这些数据 **不是来自客户端**，但你需要将其存储在数据库中时，这特别有用。例如，存储 **哈希密码**。

## 密码哈希

假设我们系统中的每个英雄都有一个 **密码**。

我们绝不能将密码以明文形式存储在数据库中，而应该只存储其 **哈希版本**。

“**哈希**”是指将某些内容（在此情况下为密码）转换为一串字节（即一个字符串），看起来像是乱码。

每次你传递完全相同的内容（即完全相同的密码），你会得到完全相同的乱码。

但你 **无法将乱码** 从 **转换回密码**。

### 为什么使用密码哈希

如果你的数据库被盗，盗贼将无法获取用户的 **明文密码**，只能拿到哈希值。

因此，盗贼将无法尝试将该密码用于另一个系统（因为许多用户在各处使用相同的密码，这将非常危险）。

/// tip

你可以使用 <a href="https://passlib.readthedocs.io/en/stable/" class="external-link" target="_blank">passlib</a> 来哈希密码。

在这个示例中，我们将使用一个伪哈希函数来专注于数据变更。🤡

///

## 使用附加数据更新模型

`Hero` 表模型现在将存储一个新的字段 `hashed_password`。

而 `HeroCreate` 和 `HeroUpdate` 的数据模型也将增加一个新的字段 `password`，用于包含客户端发送的明文密码。

//// tab | Python 3.10+

```Python hl_lines="11  15  26"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py[ln:5-28]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11  15  26"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py[ln:7-30]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11  15  26"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002.py[ln:7-30]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002.py!}
```

////

///

当客户端创建一个新英雄时，他们会在请求体中发送 `password` 字段。

当他们更新一个英雄时，也可以在请求体中发送 `password` 字段来更新密码。

## 哈希密码

应用程序将使用 `HeroCreate` 模型接收来自客户端的数据。

这个模型包含了明文密码的 `password` 字段，而我们不能直接使用这个密码。因此，我们需要从中生成一个哈希值。

//// tab | Python 3.10+

```Python hl_lines="11"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py[ln:55-57]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py[ln:44-46]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py[ln:57-59]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002.py[ln:44-46]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/update/tutorial002.py[ln:57-59]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002.py!}
```

////

///

## 使用附加数据创建对象

现在我们需要创建数据库中的英雄。

在之前的示例中，我们使用了类似这样的代码：

```Python
db_hero = Hero.model_validate(hero)
```

这将从请求中接收到的 `HeroCreate`（数据模型）对象创建一个 `Hero`（表模型）对象。

这很好……但由于 `Hero` 没有 `password` 字段，它不会从包含该字段的 `HeroCreate` 对象中提取它。

`Hero` 实际上有一个 `hashed_password` 字段，但我们没有提供它。我们需要一种方式来提供它……

### 字典更新

让我们暂停一下，检查一下，当处理字典时，有一种方法可以用另一个字典中的附加数据来 `update` 字典，类似这样：

```Python hl_lines="14"
db_user_dict = {
    "name": "Deadpond",
    "secret_name": "Dive Wilson",
    "age": None,
}

hashed_password = "fakehashedpassword"

extra_data = {
    "hashed_password": hashed_password,
    "age": 32,
}

db_user_dict.update(extra_data)

print(db_user_dict)

# {
#     "name": "Deadpond",
#     "secret_name": "Dive Wilson",
#     "age": 32,
#     "hashed_password": "fakehashedpassword",
# }
```

这个 `update` 方法允许我们用另一个字典中的数据添加和覆盖原始字典中的内容。

现在，`db_user_dict` 更新了 `age` 字段，值为 `32`，而不是 `None`，更重要的是，**它有了新的 `hashed_password` 字段**。

### 使用附加数据创建模型对象

类似于字典中的 `update` 方法，**SQLModel** 模型在 `Hero.model_validate()` 中也有一个 `update` 参数，它接受一个包含附加数据的字典，或者是应该优先使用的数据：

//// tab | Python 3.10+

```Python hl_lines="8"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py[ln:55-64]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py[ln:57-66]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002.py[ln:57-66]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002.py!}
```

////

///

现在，`db_hero`（即 *表模型* `Hero`）将从 `hero`（即 *数据模型* `HeroCreate`）中提取其值，然后它将使用来自字典 `extra_data` 的附加数据 **更新** 其值。

它只会采用 `Hero` 中定义的字段，因此 **不会获取 `HeroCreate` 中的 `password`**。它还将 **从传递给 `update` 参数的字典中获取其值**，在这种情况下为 `hashed_password`。

如果 `hero` 和 `extra_data` 中都有某个字段，**传递给 `update` 的 `extra_data` 中的值将优先**。

## 使用附加数据更新

现在假设我们要 **更新一个已经存在于数据库中的英雄**。

与之前相同，为了避免删除现有数据，我们在调用 `hero.model_dump()` 时将使用 `exclude_unset=True`，以仅获取客户端发送的数据的字典。

//// tab | Python 3.10+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py[ln:83-89]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py[ln:85-91]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002.py[ln:85-91]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002.py!}
```

////

///

现在，这个 `hero_data` 字典可能包含一个 `password` 字段。我们需要检查它，如果存在，就需要生成 `hashed_password`。

然后，我们可以将该 `hashed_password` 放入字典中。

接着，我们可以使用 `db_hero.sqlmodel_update()` 方法更新 `db_hero` 对象。

该方法接受一个模型对象或包含要更新的对象数据的字典，并且还有一个 **附加的 `update` 参数**，用于传递附加数据。

//// tab | Python 3.10+

```Python hl_lines="15"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py[ln:83-99]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="15"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py[ln:85-101]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="15"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial002.py[ln:85-101]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial002.py!}
```

////

///

/// tip

`db_hero.sqlmodel_update()` 方法是在 SQLModel 0.0.16 中添加的。😎

///

## 小结

你可以在 `Hero.model_validate()` 中使用 `update` 参数，在创建新对象时提供附加数据；并且可以在更新现有对象时，使用 `Hero.sqlmodel_update()` 提供附加数据。🤓