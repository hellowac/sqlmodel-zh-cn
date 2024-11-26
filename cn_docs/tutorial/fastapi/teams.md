# FastAPI 团队路径操作 - 其他模型

现在，让我们更新 **FastAPI** 应用程序，以处理团队的数据。

这与我们为英雄所做的非常相似，因此我们将在这里快速介绍。

我们将使用与之前示例中相同的模型，包含 **关系属性** 等。

## 添加团队模型

让我们添加团队的模型。

这与我们为英雄所做的过程相同，首先是基础模型、**表模型**，以及一些其他的 **数据模型**。

我们有一个 `TeamBase` **数据模型**，然后从它继承出 `Team` **表模型**。

接着，我们还从 `TeamBase` 继承出 `TeamCreate` 和 `TeamPublic` **数据模型**。

同时，我们还创建了一个 `TeamUpdate` **数据模型**。

//// tab | Python 3.10+

```Python hl_lines="5-7  10-13  16-17  20-21  24-26"
{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py[ln:1-26]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="7-9  12-15  18-19  22-23  26-28"
{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py[ln:1-28]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="7-9  12-15  18-19  22-23  26-28"
{!./docs_src/tutorial/fastapi/teams/tutorial001.py[ln:1-28]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001.py!}
```

////

///

现在，我们还具有了 **关系属性**。🎉

接下来，让我们更新 `Hero` 模型。

## 更新英雄模型

//// tab | Python 3.10+

```Python hl_lines="3-8  11-14  17-18  21-22  25-29"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py[ln:29-55]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3-8  11-14  17-18  21-22  25-29"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py[ln:31-57]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-8  11-14  17-18  21-22  25-29"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001.py[ln:31-57]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001.py!}
```

////

///

现在我们在英雄模型中加入了 `team_id`。

注意，我们可以在 `HeroBase` 中声明 `team_id`，因为它可以被所有模型重用，且在所有情况下它都是一个可选的整数。

即使 `HeroBase` *不是* 一个 **表模型**，我们也可以在其中声明 `team_id`，并使用 `foreign key` 参数。对于继承自 `HeroBase` 的大多数模型来说，这不会产生任何影响，但在 **表模型** `Hero` 中，它将用于告诉 **SQLModel** 这是一个指向该表的 **外键**。

## 关系属性

请注意，**关系属性**（使用 `Relationship()` 的那些）**仅**存在于 **表模型** 中，因为这些模型由 **SQLModel** 与 SQLAlchemy 处理，且当我们访问它们时，能够自动从数据库中获取数据。

//// tab | Python 3.10+

```Python hl_lines="11  38"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py[ln:5-55]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11  38"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py[ln:7-57]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11  38"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001.py[ln:7-57]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001.py!}
```

////

///

## 团队的路径操作

现在让我们为团队添加 **路径操作**。

这些操作与我们之前为 **英雄** 创建的 **路径操作** 相似，因此我们不需要详细讲解每一个操作，直接来看代码吧。

//// tab | Python 3.10+

```Python hl_lines="3-9  12-20  23-28  31-47  50-57"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py[ln:136-190]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3-9  12-20  23-28  31-47  50-57"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py[ln:138-192]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-9  12-20  23-28  31-47  50-57"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/teams/tutorial001.py[ln:138-192]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/teams/tutorial001.py!}
```

////

///

## 使用关系属性

到目前为止，我们实际上还没有使用 **关系属性**，但我们可以在代码中访问它们。

在下一章节中，我们将进一步操作这些关系属性。

## 查看文档 UI

现在我们可以查看自动生成的文档 UI，查看所有关于英雄和团队的 **路径操作**。

<img class="shadow" alt="交互式 API 文档 UI" src="/img/tutorial/fastapi/teams/image01.png">

## 总结

我们可以使用相同的模式向 **FastAPI** 应用程序中添加更多模型和 API **路径操作**。🎉
