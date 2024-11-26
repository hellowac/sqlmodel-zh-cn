# 使用 FastAPI 更新数据

现在，让我们来看一下如何使用 **FastAPI** 的 *路径操作* 来更新数据库中的数据。

## `HeroUpdate` 模型

我们希望客户端能够更新英雄的 `name`、`secret_name` 和 `age`。

但是，我们不希望他们在更新单个字段时必须重新提供所有数据。

因此，我们需要将这些字段 **标记为可选**。

由于 `HeroBase` 中的一些字段是 *必需的*，而不是可选的，因此我们将需要 **创建一个新模型**。

/// tip

这是一个可能更适合使用 **独立模型** 的情况，而不是试图通过创建复杂的继承模型树来解决问题。

因为每个字段 **实际上是不同的**（我们只是将其更改为 `Optional`，但这已经使它不同），因此将它们放入自己的模型中是合理的。

///

所以，我们来创建这个新的 `HeroUpdate` 模型：

//// tab | Python 3.10+

```Python hl_lines="21-24"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py[ln:5-26]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="21-24"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py[ln:7-28]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="21-24"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001.py[ln:7-28]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001.py!}
```

////

///

这几乎与 `HeroBase` 相同，但所有字段都是可选的，因此我们不能简单地从 `HeroBase` 继承。

## 创建更新路径操作

现在，让我们在 *路径操作* 中使用这个模型来更新英雄。

我们将使用 `PATCH` HTTP 操作。这用于 **部分更新数据**，正是我们所做的操作。

//// tab | Python 3.10+

```Python hl_lines="3-4"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py[ln:74-89]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3-4"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py[ln:76-91]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-4"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001.py[ln:76-91]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001.py!}
```

////

///

我们还从 *路径参数* 和请求体中读取 `hero_id` 和 `HeroUpdate`。

### 读取现有的英雄

我们通过 **hero_id** 获取想要更新的英雄的 **ID**。

因此，我们需要使用与 **读取单个英雄** 时相同的逻辑从数据库中读取英雄，检查其是否存在，如果不存在，则可能会抛出错误给客户端，等等。

//// tab | Python 3.10+

```Python hl_lines="6-8"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py[ln:74-89]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="6-8"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py[ln:76-91]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="6-8"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001.py[ln:76-91]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001.py!}
```

////

///

### 获取新的数据

`HeroUpdate` 模型包含所有具有 **默认值** 的字段，因为它们都是可选的，这正是我们所需要的。

但这也意味着，如果我们直接调用 `hero.model_dump()`，我们会得到一个字典，其中可能包含多个或所有默认值字段，例如：

```Python
{
    "name": None,
    "secret_name": None,
    "age": None,
}
```

然后，如果我们使用这些数据更新数据库中的英雄，我们将会删除任何现有的值，而这可能 **不是客户端的意图**。

幸运的是，Pydantic 模型（以及 SQLModel 模型）提供了一个参数，我们可以在 `.model_dump()` 方法中传递：`exclude_unset=True`。

这告诉 Pydantic **不包括** 客户端 **没有发送** 的值。换句话说，它只会 **包括** 客户端 **发送的值**。

因此，如果客户端发送一个没有值的 JSON：

```JSON
{}
```

那么，使用 `hero.model_dump(exclude_unset=True)` 获取的字典将是：

```Python
{}
```

但如果客户端发送的 JSON 包含：

```JSON
{
    "name": "Deadpuddle"
}
```

那么，使用 `hero.model_dump(exclude_unset=True)` 获取的字典将是：

```Python
{
    "name": "Deadpuddle"
}
```

然后，我们使用这个数据来更新客户端实际发送的数据：

//// tab | Python 3.10+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py[ln:74-89]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py[ln:76-91]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001.py[ln:76-91]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001.py!}
```

////

///

/// tip

在 SQLModel 0.0.14 之前，方法名是 `hero.dict(exclude_unset=True)`，但为了与 Pydantic v2 保持一致，它被重命名为 `hero.model_dump(exclude_unset=True)`。

///

## 在数据库中更新英雄

现在我们已经有了 **客户端发送的数据字典**，我们可以使用 `db_hero.sqlmodel_update()` 方法来更新对象 `db_hero`。

//// tab | Python 3.10+

```Python hl_lines="10"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py[ln:74-89]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="10"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py[ln:76-91]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="10"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/update/tutorial001.py[ln:76-91]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/update/tutorial001.py!}
```

////

///

/// tip

`db_hero.sqlmodel_update()` 方法是在 SQLModel 0.0.16 中新增的。🤓

在那之前，你需要手动获取值并使用 `setattr()` 设置它们。

///

`db_hero.sqlmodel_update()` 方法接受一个模型对象或字典作为参数。

对于 **原始** 模型对象（在此示例中为 `db_hero`）中的每个字段，它会检查该字段是否在 **参数**（在此示例中为 `hero_data`）中存在，然后使用提供的值进行更新。

## 移除字段

这里有一个附加功能。🎁

当获取客户端发送的数据字典时，我们只包含 **客户端实际发送的内容**。

这听起来很简单，但它有一些额外的细节，成为了 **很棒的特性**。✨

我们 **不是简单地省略** 具有 **默认值** 的数据。

我们也 **不是简单地省略** 任何为 `None` 的数据。

这意味着，如果数据库中的模型 **有一个与默认值不同的值**，客户端可以 **将其重置为与默认值相同的值**，甚至是 `None`，我们 **仍然会注意到** 并 **根据需要更新** 它。🤯🚀

因此，如果客户端想有意删除英雄的 `age` 字段，他们只需发送一个包含以下内容的 JSON：

```JSON
{
    "age": null
}
```

然后，当使用 `hero.model_dump(exclude_unset=True)` 获取数据时，我们会得到：

```Python
{
    "age": None
}
```

因此，我们会使用该值并将数据库中的 `age` 更新为 `None`，**正如客户端所希望的那样**。

请注意，`age` 这里是 `None`，并且 **我们仍然检测到了这一点**。

另外，`name` 甚至没有发送，我们并没有 *错误地* 将其设置为 `None` 或其他内容。我们只是没有更改它，因为客户端没有发送它，所以我们 **完全没有问题**，即使在这些边缘情况中。✨

这些是使用 Pydantic 和 SQLModel 的一些优势。🎉

## 总结

通过在 SQLModel 模型（和 Pydantic 模型）中使用 `.model_dump(exclude_unset=True)`，我们可以轻松、**正确地** 更新数据，即使是在 **边缘情况** 下。😎