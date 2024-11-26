# 创建和更新关系

现在让我们看看如何使用这些新的 **关系属性** 来创建具有关系的数据。✨

## 使用字段创建实例

让我们回顾一下之前用来创建英雄和队伍的旧代码：

//// tab | Python 3.10+

```Python hl_lines="9  12  18  24"
# 代码上面部分省略 👆

{!./docs_src/tutorial/connect/insert/tutorial001_py310.py[ln:29-58]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9  12  18  24"
# 代码上面部分省略 👆

{!./docs_src/tutorial/connect/insert/tutorial001.py[ln:31-60]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/connect/insert/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/connect/insert/tutorial001.py!}
```

////

///

这里有几个 **注意事项** 。

首先，我们 **创建** 了一些 `Team` 实例对象。在创建 `Hero` 实例时，我们希望使用这些队伍的 ID，并将其填充到 `team_id` 字段中。

但是，模型实例在数据库中 **不会生成 ID** ，直到我们将它们通过 `add` 和 `commit` 添加到 **会话** 中。在此之前，它们的 ID 是 `None`，而我们希望使用实际的 ID。

因此，我们必须先 `add` 它们，并提交会话，之后才能开始创建 `Hero` 实例，以便能够 **使用它们的 ID** 。

接着，在创建 `Hero` 实例时，我们会使用这些 ID。我们将新的英雄添加到会话中，然后提交。

所以，我们 **提交了两次** 。我们需要记得先 `add` 某些对象，然后再 `commit`，并且要按照正确的顺序进行操作，否则我们可能会使用一个当前为 `None` 的 `team.id`，因为它尚未保存。

这是这些 **关系属性** 帮助的第一个地方。🤓

## 使用关系属性创建实例

现在让我们使用新的、闪亮的 `Relationship` 属性来做同样的事情：

//// tab | Python 3.10+

```Python hl_lines="9  12  18"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py310.py[ln:32-55]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9  12  18"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py39.py[ln:34-57]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9  12  18"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001.py[ln:34-57]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001.py!}
```

////

///

现在，我们可以创建 `Team` 实例，并直接将它们传递给创建 `Hero` 实例时的 `team` 参数，例如使用 `team=team_preventers`，而不是 `team_id=team_preventers.id`。

得益于 SQLAlchemy 和其底层的工作方式，这些队伍甚至不需要提前有 ID，但因为我们将整个对象赋给每个英雄，它们 **会自动在数据库中创建** ，并生成自动 ID，接着会将该 ID 设置到每个英雄行的 `team_id` 列中。

事实上，现在我们甚至不需要使用 `session.add(team)` 显式将队伍添加到会话中，因为这些 `Team` 实例已经 **与我们添加到会话中的英雄** 关联。

SQLAlchemy 知道，它还需要在下次提交时将这些队伍包含在内，以便正确保存英雄数据。

然后，正如你所看到的，我们只需要做一次 `commit()`。

## 分配关系

就像我们可以将一个整数值（`team.id`）分配给 `hero.team_id` 一样，我们也可以将 `Team` 实例分配给 `hero.team`：

//// tab | Python 3.10+

```Python hl_lines="8"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py[ln:32-33]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py[ln:57-61]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py[ln:34-35]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py[ln:59-63]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py[ln:34-35]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py[ln:59-63]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py!}
```

////

///

## 创建包含英雄的团队

之前，我们创建了 `Team` 实例并将其作为 `team=` 参数传递给 `Hero` 实例。

我们也可以先创建 `Hero` 实例，然后在创建 `Team` 实例时，将它们作为 `heroes=` 参数（它接受一个列表）传递：

//// tab | Python 3.10+

```Python hl_lines="13  15-16"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py[ln:32-33]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py[ln:63-73]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="13  15-16"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py[ln:34-35]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py[ln:65-75]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="13  15-16"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py[ln:34-35]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py[ln:65-75]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py!}
```

////

///

在这里，我们首先创建了两个英雄，**Black Lion** 和 **Princess Sure-E**，然后将它们传递给 `heroes` 参数。

请注意，与之前一样，我们只需要将 `Team` 实例添加到会话中，并且由于这些英雄与它关联，当我们调用 `commit` 时，它们也会被自动保存。

## 在“多”方包含关系对象

我们之前提到这是一个 **多对一** 关系，因为可以有 **多个** 英雄属于 **一个** 团队。

我们也可以在 **多** 方使用这些关系属性来连接数据。

由于 `team.heroes` 属性表现得像一个列表，我们可以直接向其中添加数据。

让我们创建一些新的英雄并将它们添加到 `team_preventers.heroes` 列表属性中：

//// tab | Python 3.10+

```Python hl_lines="14-18"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py[ln:32-33]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py[ln:75-91]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="14-18"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py[ln:34-35]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py[ln:77-93]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="14-18"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py[ln:34-35]!}

        # 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py[ln:77-93]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/create_and_update_relationships/tutorial001.py!}
```

////

///

`team_preventers.heroes` 属性表现得像一个列表，但它是一个特殊类型的列表，因为当我们修改它，向其中添加英雄时， **SQLModel** （实际上是 SQLAlchemy）会 **跟踪** 必须在数据库中执行的相应更改。

然后，我们将团队 `add()` 到会话中并执行 `commit()`。

和之前一样，我们甚至不需要将独立的英雄通过 `add()` 方法添加到会话中，因为它们已经与团队 **关联** 。

## 总结

我们可以使用常规的 Python 对象和属性，通过这些 **关系属性** 来创建和更新数据连接。😎

接下来，我们将看到如何使用这些关系属性来读取关联数据。🤝
