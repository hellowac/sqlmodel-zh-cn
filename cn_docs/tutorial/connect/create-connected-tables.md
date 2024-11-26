# 创建连接表格

现在我们将处理存储在不同表格中的 **连接** 数据。

第一步是创建多个表格并将它们连接起来，这样一个表格中的每一行就可以引用另一个表格中的某一行。

我们之前一直在处理单一表格 `hero` 中的英雄数据。现在，我们来添加一个表格 `team`。

`team` 表格的结构如下所示：

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

为了将它们连接起来，我们将在 `hero` 表中添加另一个列 `team_id`，通过该 ID 指向每个团队：

<table>
<tr>
<th>id</th><th>name</th><th>secret_name</th><th>age</th><th>team_id ✨</th>
</tr>
<tr>
<td>1</td><td>Deadpond</td><td>Dive Wilson</td><td>null</td><td>2 ✨</td>
</tr>
<tr>
<td>2</td><td>Spider-Boy</td><td>Pedro Parqueador</td><td>null</td><td>1 ✨</td>
</tr>
<tr>
<td>3</td><td>Rusty-Man</td><td>Tommy Sharp</td><td>48</td><td>1 ✨</td>
</tr>
</table>

这样，`hero` 表中的每一行就可以指向 `team` 表中的某一行：

<img alt="table relationships" src="../../../img/databases/relationships.svg">

## 一对多和多对一

在这里，我们创建了一个连接的数据关系，其中 **一个** 团队可以拥有 **多个** 英雄。所以，这通常被称为 **一对多** 或 **多对一** 关系。

从英雄的角度来看，**多个** 英雄可以属于 **一个** 团队，这就是 **多对一** 关系。

这是最常见的关系类型，因此我们从这一种关系开始。但也有 **多对多** 和 **一对一** 关系。

## 在代码中创建表格

### 创建 `team` 表格

让我们从在代码中创建表格开始。

导入我们需要的 `sqlmodel` 模块，并创建一个新的 `Team` 模型：

//// tab | Python 3.10+

```Python hl_lines="4-7"
{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py[ln:1-7]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="6-9"
{!./docs_src/tutorial/connect/create_tables/tutorial001.py[ln:1-9]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001.py!}
```

////

///

这与我们之前对 `Hero` 模型的操作非常相似。

`Team` 模型会自动对应一个名为 `"team"` 的表，并且会包含以下列：

* `id`：主键，由数据库自动生成
* `name`：团队的名称
    * 我们还告诉 **SQLModel** 为该列创建索引
* `headquarters`：团队的总部

最后，我们在配置中将其标记为一个表格。

### 创建新的 `hero` 表格

现在我们来创建 `hero` 表格。

这与我们之前使用的模型相同，只是我们添加了新的 `team_id` 列：

//// tab | Python 3.10+

```Python hl_lines="16"
{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py[ln:1-16]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="18"
{!./docs_src/tutorial/connect/create_tables/tutorial001.py[ln:1-18]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001.py!}
```

////

///

大部分内容应该都很熟悉：

列的名称将是 `team_id`。它将是一个整数，并且在数据库中可能是 `NULL`（在 Python 中是 `None`），因为可能有些英雄不属于任何团队。

我们在 `Field()` 中为 `team_id` 添加了默认值 `None`，这样在创建英雄时就不需要显式地传递 `team_id=None`。

现在，这里是新的部分：

在 `Field()` 中，我们传递了 `foreign_key="team.id"` 参数。这告诉数据库，`team_id` 列是指向 `team` 表的外键。 **外键** 意味着这个列将包含用来标识 **外部** 表中某行的 **主键** 。

该列 `team_id` 中的值将是 `team` 表中 `id` 列的某一行的整数值。正是这个值将连接两个表格。

#### `foreign_key` 的值

注意，`foreign_key` 是一个字符串。

字符串中包含了 **表名**，然后是一个点，再接着是 **列名**。

这表示数据库中的 **表名**，因此是 `"team"`，而不是 **模型** 类 `Team`（大写字母 `T`）。

如果你使用了自定义的表名，就应该使用那个自定义的表名。

/// tip

你可以在高级用户指南中了解如何为模型设置自定义表名。

///

### 创建表格

现在我们可以像之前一样添加代码来创建引擎，并创建一个函数来创建表格：

//// tab | Python 3.10+

```Python hl_lines="3-4  6  9-10"
# 代码省略 👆

{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py[ln:19-26]!}
```

////

//// tab | Python 3.7+

```Python hl_lines="3-4  6  9-10"
# 代码省略 👆

{!./docs_src/tutorial/connect/create_tables/tutorial001.py[ln:21-28]!}
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001.py!}
```

////

///

与之前一样，我们将从另一个函数 `main()` 调用此函数，并将 `main()` 函数添加到文件的主块中：

//// tab | Python 3.10+

```Python hl_lines="3-4  7-8"
# 代码省略 👆

{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py[ln:29-34]!}
```

////

//// tab | Python 3.7+

```Python hl_lines="3-4  7-8"
# 代码省略 👆

{!./docs_src/tutorial/connect/create_tables/tutorial001.py[ln:31-36]!}
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/connect/create_tables/tutorial001.py!}
```

////

///

## 运行代码

/// tip

在运行代码之前，请确保删除文件 `database.db`，以确保从头开始。

///

如果我们运行到目前为止的代码，它将创建数据库文件 `database.db`，并在其中创建我们刚刚定义的 `team` 和 `hero` 表：

<div class="termy">

```console
$ python app.py

// 自动开始一个新的事务
INFO Engine BEGIN (implicit)

// 检查表格是否已经存在
INFO Engine PRAGMA main.table_info("team")
INFO Engine [raw sql] ()
INFO Engine PRAGMA temp.table_info("team")
INFO Engine [raw sql] ()
INFO Engine PRAGMA main.table_info("hero")
INFO Engine [raw sql] ()
INFO Engine PRAGMA temp.table_info("hero")
INFO Engine [raw sql] ()

// 创建表格
INFO Engine
CREATE TABLE team (
        id INTEGER,
        name VARCHAR NOT NULL,
        headquarters VARCHAR NOT NULL,
        PRIMARY KEY (id)
)


INFO Engine [no key 0.00010s] ()
INFO Engine
CREATE TABLE hero (
        id INTEGER,
        name VARCHAR NOT NULL,
        secret_name VARCHAR NOT NULL,
        age INTEGER,
        team_id INTEGER,
        PRIMARY KEY (id),
        FOREIGN KEY(team_id) REFERENCES team (id)
)


INFO Engine [no key 0.00026s] ()
INFO Engine COMMIT
```

</div>

## 在 SQL 中创建表格

让我们看看生成的 SQL 代码。

如我们之前所见，这些 `VARCHAR` 列在 SQLite 中会被转换为 `TEXT` 类型，这是我们用来进行实验的数据库。

所以，第一条 SQL 语句也可以写成：

```SQL
CREATE TABLE team (
    id INTEGER,
    name TEXT NOT NULL,
    headquarters TEXT NOT NULL,
    PRIMARY KEY (id)
)
```

第二个表格则可以写成：

```SQL hl_lines="8"
CREATE TABLE hero (
    id INTEGER,
    name TEXT NOT NULL,
    secret_name TEXT NOT NULL,
    age INTEGER,
    team_id INTEGER,
    PRIMARY KEY (id),
    FOREIGN KEY(team_id) REFERENCES team (id)
)
```

唯一的新部分是 `FOREIGN KEY` 这一行，如你所见，它告诉数据库该表中的哪个列是外键（`team_id`），它引用了哪个（外部）表（`team`），以及那个表中的哪个列是连接行的关键（`id`）。

你可以在 **DB Browser for SQLite** 中自由地进行实验。

## 小结

使用 **SQLModel** 时，在大多数情况下，你只需要在 `Field()` 中添加一个字段（列）并指定一个 `foreign_key`，它指向另一个表和列，从而连接两个表。

现在我们已经创建并连接了表格，接下来让我们在下一章节中创建一些数据行。🚀
