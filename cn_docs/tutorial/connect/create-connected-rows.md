# 创建并连接数据行

现在我们将为每个表格 **创建数据行**。✨

`team` 表格将如下所示：

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

在我们完成本章中的数据操作后，`hero` 表格将如下所示：

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
<td>3</td><td>Spider-Boy</td><td>Pedro Parqueador</td><td>null</td><td>null</td>
</tr>
</table>

`hero` 表格中的每一行都将指向 `team` 表格中的一行：

<img alt="table relationships" src="/img/tutorial/relationships/select/relationships2.svg">

/// info

我们稍后会更新 **Spider-Boy**，将他也添加到 **Preventers** 团队，但现在还不进行。

///

我们将继续使用前面示例中的代码，并在此基础上进行扩展。

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

在运行示例之前，请确保删除 `database.db` 文件，以确保得到相同的结果。

## 使用 **SQLModel** 为团队创建数据行

让我们做和之前一样的操作，定义一个 `create_heroes()` 函数，在其中创建我们的英雄。

现在我们也将在这里创建团队。🎉

首先，创建两个团队：

//// tab | Python 3.10+

```Python hl_lines="3-9"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001_py310.py[ln:29-35]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-9"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001.py[ln:31-37]!}

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

这应该已经看起来很熟悉了。

我们在一个 `with` 块中启动一个 **session**，使用之前创建的 **engine**。

然后我们创建 `Team` 模型类的两个实例。

接下来，我们将这些对象添加到 **session** 中。

最后，我们 **commit** 该 session，将更改保存到数据库中。

## 将其添加到 Main 函数

别忘了将这个函数 `create_heroes()` 添加到 `main()` 函数中，这样我们就能在从命令行调用程序时运行它：

//// tab | Python 3.10+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001_py310.py[ln:61-63]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001.py[ln:63-65]!}

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

## 运行代码

如果我们运行到目前为止的代码，它将输出：

<div class="termy">

```console
$ python app.py

// Previous output omitted 😉

// 自动启动事务
INFO Engine BEGIN (implicit)
// 将团队添加到数据库
INFO Engine INSERT INTO team (name, headquarters) VALUES (?, ?)
INFO Engine [generated in 0.00050s] ('Preventers', 'Sharp Tower')
INFO Engine INSERT INTO team (name, headquarters) VALUES (?, ?)
INFO Engine [cached since 0.002324s ago] ('Z-Force', 'Sister Margaret's Bar')
INFO Engine COMMIT
```

</div>

你可以在输出中看到，它使用了常见的 SQL `INSERT` 语句来创建数据行。

## 在代码中创建英雄行

现在我们开始创建一个英雄对象。

由于 `Hero` 类模型现在有一个字段（列，属性）`team_id`，我们可以通过使用之前创建的 `Team` 对象的 ID 字段来设置它：

//// tab | Python 3.10+

```Python hl_lines="12"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001_py310.py[ln:29-39]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="12"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001.py[ln:31-41]!}

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

到目前为止，我们还没有将这个英雄提交到数据库，但已经有几个需要**注意**的地方。

如果数据库中已经有一些团队，我们甚至不知道**哪个 ID**会被数据库自动分配给每个团队。例如，我们不能仅仅猜测是 `1` 还是 `2`。

但是一旦团队创建并提交到数据库，我们就可以通过访问对象的 `id` 字段来获取该 ID。

访问刚刚提交的模型中的属性，例如 `team_z_force.id`，会自动**触发数据的刷新**，并从数据库中获取该团队的数据，然后将字段值暴露出来。

因此，即使我们还没有提交这个英雄，只是因为我们使用了 `team_z_force.id`，这将触发 SQL 查询来获取该团队的数据。

单单这一行就会生成如下输出：

```
INFO Engine BEGIN (implicit)
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [generated in 0.00025s] (2,)
```

接下来，我们再创建两个英雄：

//// tab | Python 3.10+

```Python hl_lines="14-20"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001_py310.py[ln:29-50]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="14-20"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001.py[ln:31-52]!}

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

在创建 `hero_rusty_man` 时，我们访问了 `team_preventers.id`，因此也会触发数据的刷新，生成如下输出：

```
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [cached since 0.001795s ago] (1,)
```

还有一些值得注意的地方。我们将 `team_id` 标记为 `Optional[int]`，意味着它在数据库中可以是 `NULL`（在 Python 中是 `None`）。

这意味着一个英雄不必属于任何团队。在这个例子中，**Spider-Boy** 就没有所属的团队。

接下来，我们只需提交这些更改以保存到数据库，这将生成如下输出：

```
INFO Engine INSERT INTO hero (name, secret_name, age, team_id) VALUES (?, ?, ?, ?)
INFO Engine [generated in 0.00022s] ('Deadpond', 'Dive Wilson', None, 2)
INFO Engine INSERT INTO hero (name, secret_name, age, team_id) VALUES (?, ?, ?, ?)
INFO Engine [cached since 0.0007987s ago] ('Rusty-Man', 'Tommy Sharp', 48, 1)
INFO Engine INSERT INTO hero (name, secret_name, age, team_id) VALUES (?, ?, ?, ?)
INFO Engine [cached since 0.001095s ago] ('Spider-Boy', 'Pedro Parqueador', None, None)
INFO Engine COMMIT
```

## 刷新并打印英雄

现在让我们刷新并打印这些新创建的英雄，以查看它们指向团队的新 ID：

//// tab | Python 3.10+

```Python hl_lines="26-28 30-32"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001_py310.py[ln:29-58]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="26-28 30-32"
# Code above omitted 👆

{!./docs_src/tutorial/connect/insert/tutorial001.py[ln:31-60]!}

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

如果我们在命令行中执行它，它将输出：

<div class="termy">

```console
$ python app.py

// Previous output omitted 😉

// Automatically start a transaction
INFO Engine BEGIN (implicit)

// Refresh the first hero
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age, hero.team_id
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00021s] (1,)
// Refresh the second hero
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age, hero.team_id
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.001575s ago] (2,)
// Refresh the third hero
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age, hero.team_id
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.002518s ago] (3,)

// Print the heroes
Created hero: id=1 secret_name='Dive Wilson' team_id=2 name='Deadpond' age=None
Created hero: id=2 secret_name='Tommy Sharp' team_id=1 name='Rusty-Man' age=48
Created hero: id=3 secret_name='Pedro Parqueador' team_id=None name='Spider-Boy' age=None
```

</div>

它们现在已经有了 `team_id`，太好了！

## 关系

SQL 数据库中的关系就是通过在 **一张表中的列** 引用 **另一张表中的列** 的值来建立的。

在这里，我们把它们当作 **列字段** 来处理，这其实就是它们在 SQL 数据库中的实际表现。

但是，在本教程的后面章节中，您将学习到关于 **关系属性** 的内容，这将使在代码中使用这些关系变得更加简单。✨