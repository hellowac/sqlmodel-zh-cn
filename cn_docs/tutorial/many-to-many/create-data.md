# 使用多对多关系创建数据

让我们从之前的内容继续，创建一些数据。

我们将为这个通过链接表建立的 **多对多** 关系创建数据：

<img alt="many-to-many table relationships" src="../../../img/tutorial/many-to-many/many-to-many.svg">

我们将在之前代码的基础上继续。

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001.py!}
```

////

///

## 创建英雄

和之前一样，我们会创建一个名为 `create_heroes()` 的函数，并在其中创建一些团队和英雄：

//// tab | Python 3.10+

```Python hl_lines="11"
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001_py310.py[ln:36-54]!}

# 下方代码已省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11"
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001_py39.py[ln:42-60]!}

# 下方代码已省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11"
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001.py[ln:42-60]!}

# 下方代码已省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001.py!}
```

////

///

这与之前的操作非常相似。

我们创建了两个团队，然后创建了三个英雄。

唯一的新细节是，我们不再使用参数 `team`，而是使用 `teams`，因为这是新的 **关系属性** 的名称。更重要的是，我们传递了一个 **团队列表**（即使它只有一个团队）。

请注意，**Deadpond** 现在属于两个团队了！

## 提交、刷新并打印

现在，让我们像之前一样，`commit` **会话**，`refresh` 数据，并打印它：

//// tab | Python 3.10+

```Python hl_lines="22-25  27-29  31-36"
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001_py310.py[ln:36-69]!}

# 下方代码已省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="22-25  27-29  31-36"
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001_py39.py[ln:42-75]!}

# 下方代码已省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="22-25  27-29  31-36"
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001.py[ln:42-75]!}

# 下方代码已省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001.py!}
```

////

///

## 添加到主函数

和之前一样，将 `create_heroes()` 函数添加到 `main()` 函数中，以确保在从命令行运行该程序时调用它：

//// tab | Python 3.10+

```Python
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001_py310.py[ln:72-74]!}

# 下方代码已省略 👇
```

////

//// tab | Python 3.9+

```Python
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001_py39.py[ln:78-80]!}

# 下方代码已省略 👇
```

////

//// tab | Python 3.7+

```Python
# 上方代码已省略 👆

{!./docs_src/tutorial/many_to_many/tutorial001.py[ln:78-80]!}

# 下方代码已省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial001.py!}
```

////

///

## 运行程序

如果从命令行运行程序，输出将如下所示：

<div class="termy">

```console
$ python app.py

// 之前的输出已省略 🙈

// 自动开始一个新事务
INFO Engine BEGIN (implicit)
// 首先插入英雄数据
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [generated in 0.00041s] ('Deadpond', 'Dive Wilson', None)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.001942s ago] ('Rusty-Man', 'Tommy Sharp', 48)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.002541s ago] ('Spider-Boy', 'Pedro Parqueador', None)
// 然后插入队伍数据
INFO Engine INSERT INTO team (name, headquarters) VALUES (?, ?)
INFO Engine [generated in 0.00037s] ('Z-Force', 'Sister Margaret's Bar')
INFO Engine INSERT INTO team (name, headquarters) VALUES (?, ?)
INFO Engine [cached since 0.001239s ago] ('Preventers', 'Sharp Tower')
// 最后插入链接数据，以便重用已创建的 ID
INFO Engine INSERT INTO heroteamlink (team_id, hero_id) VALUES (?, ?)
INFO Engine [generated in 0.00026s] ((2, 3), (1, 1), (2, 1), (2, 2))
// 提交事务并将数据保存到数据库
INFO Engine COMMIT

// 自动开始一个新事务
INFO Engine BEGIN (implicit)
// 刷新数据
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00019s] (1,)
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.001959s ago] (2,)
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.003215s ago] (3,)

// 打印 Deadpond
Deadpond: name='Deadpond' age=None id=1 secret_name='Dive Wilson'

// 访问 `.team` 属性触发刷新
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team, heroteamlink
WHERE ? = heroteamlink.hero_id AND team.id = heroteamlink.team_id
INFO Engine [generated in 0.00025s] (1,)

// 打印 Deadpond 的队伍，2 个队伍！🎉
Deadpond teams: [Team(id=1, name='Z-Force', headquarters='Sister Margaret's Bar'), Team(id=2, name='Preventers', headquarters='Sharp Tower')]

// 打印 Rusty-Man
Rusty-Man: name='Rusty-Man' age=48 id=2 secret_name='Tommy Sharp'

// 访问 `.team` 属性触发刷新
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team, heroteamlink
WHERE ? = heroteamlink.hero_id AND team.id = heroteamlink.team_id
INFO Engine [cached since 0.001716s ago] (2,)

// 打印 Rusty-Man 的队伍，只有一个，但仍然是列表
Rusty-Man Teams: [Team(id=2, name='Preventers', headquarters='Sharp Tower')]

// 打印 Spider-Boy
Spider-Boy: name='Spider-Boy' age=None id=3 secret_name='Pedro Parqueador'

// 访问 `.team` 属性触发刷新
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team, heroteamlink
WHERE ? = heroteamlink.hero_id AND team.id = heroteamlink.team_id
INFO Engine [cached since 0.002739s ago] (3,)

// 打印 Spider-Boy 的队伍，只有一个，但仍然是列表
Spider-Boy Teams: [Team(id=2, name='Preventers', headquarters='Sharp Tower')]

// 在 `with` 块结束时，自动回滚任何先前的自动事务
INFO Engine ROLLBACK
```

</div>

## 回顾

在设置好模型链接后，使用 **关系属性** 的操作非常直观，仅需处理 Python 对象即可。✨
