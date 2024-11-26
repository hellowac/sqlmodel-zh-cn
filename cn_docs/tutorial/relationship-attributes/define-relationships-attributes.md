# 定义关系属性

现在我们终于来到了 **SQLModel** 中最激动人心的部分之一——关系属性。✨

我们目前有一个 `team` 表：

<table>
<tr>
<th>id</th><th>name</th><th>headquarters</th>
</tr>
<tr>
<td>1</td><td>Preventers</td><td>Sharp Tower</td>
</tr>
<tr>
<td>2</td><td>Z-Force</td><td>Sister Margaret's Bar</td>
</tr>
</table>

还有一个 `hero` 表：

<table>
<tr>
<th>id</th><th>name</th><th>secret_name</th><th>age</th><th>team_id</th>
</tr>
<tr>
<td>1</td><td>Deadpond</td><td>Dive Wilson</td><td>null</td><td>2</td>
</tr>
<tr>
<td>2</td><td>Rusty-Man</td><td>Tommy Sharp</td><td>48</td><td>1</td>
</tr>
<tr>
<td>3</td><td>Spider-Boy</td><td>Pedro Parqueador</td><td>null</td><td>1</td>
</tr>
</table>

现在，您已经了解了这些表在底层的工作原理，以及模型类如何表示它们，是时候添加一些便捷的功能了，这将使许多操作变得更加简洁。

## 声明关系属性

到目前为止，我们只使用了 `team_id` 列来在查询时连接表格，使用 `select()`：

//// tab | Python 3.10+

```Python hl_lines="16"
{!./docs_src/tutorial/connect/insert/tutorial001_py310.py[ln:1-16]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="18"
{!./docs_src/tutorial/connect/insert/tutorial001.py[ln:1-18]!}

# Code below omitted 👇
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

这是一个 **普通的字段** ，就像其他所有字段一样，都表示 **表中的一列**。

但是现在，让我们向这些模型类添加几个新的特殊属性——即添加 `Relationship` 属性。

首先，从 `sqlmodel` 导入 `Relationship`：

//// tab | Python 3.10+

```Python hl_lines="1"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py310.py[ln:1]!}

# Code below omitted 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py39.py[ln:1-3]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001.py[ln:1-3]!}

# Code below omitted 👇
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

接下来，使用 `Relationship` 来在模型类中声明一个新的属性：

//// tab | Python 3.10+

```Python hl_lines="9  19"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py310.py[ln:1-19]!}

# Code below omitted 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py39.py[ln:1-21]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001.py[ln:1-21]!}

# Code below omitted 👇
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

## 什么是关系属性

这些新属性与字段不同，它们**不直接代表数据库中的一列**，其值也不像整数那样是一个单一的值。它们的值是**与之相关的整个对象**。

例如，在 `Hero` 实例中，如果你调用 `hero.team`，你将得到这个英雄所属的整个 `Team` 实例对象。✨

举个例子，你可以检查一个 `hero` 是否属于某个 `team`（如果 `.team` 不为 `None`），然后打印该队伍的 `name`：

```Python
if hero.team:
    print(hero.team.name)
```

## 可选关系属性

请注意，在 `Hero` 类中，`team` 的类型注解是 `Optional[Team]`。

这意味着这个属性可以是 `None`，也可以是一个完整的 `Team` 对象。

这是因为相关的 **`team_id` 也可以是 `None`**（或者数据库中的 `NULL`）。

如果要求每个 `Hero` 实例都必须属于一个 `Team`，那么 `team_id` 的类型应该是 `int`，而不是 `Optional[int]`，它的 `Field` 应该是 `Field(foreign_key="team.id")`，而不是 `Field(default=None, foreign_key="team.id")`，而 `team` 属性则应该是 `Team` 类型，而不是 `Optional[Team]`。

## 带列表的关系属性

在 `Team` 类中，`heroes` 属性被注解为 `Hero` 对象的列表，因为这正是它所包含的内容。

**SQLModel**（实际上是 SQLAlchemy）足够智能，能够知道关系是通过 `team_id` 建立的，因为这是从 `hero` 表指向 `team` 表的外键，因此我们不需要在这里显式指定这一点。

/// tip

在接下来的章节中，我们会再次检查一些关于 `List["Hero"]` 和 `back_populates` 的内容。

但现在，首先让我们看看如何使用这些关系属性。

///

## 下一步

现在，让我们在接下来的章节中看看如何使用这些新的**关系属性**的实际例子。✨
