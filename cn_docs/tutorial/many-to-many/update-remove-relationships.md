# 更新和移除多对多关系

现在我们来学习如何更新和移除这些 **多对多** 关系。

我们将继续之前的代码。

/// details | 👀 查看完整文件预览

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

## 获取待更新的数据

现在我们来创建一个函数 `update_heroes()`。

我们将获取 **Spider-Boy** 和 **Z-Force** 团队。

由于你已经熟悉基本的操作，我们会使用 **简短版本**，在一个 Python 语句中获取这些数据。

此外，由于我们使用了 `select()`，我们需要将其导入。

//// tab | Python 3.10+

```Python hl_lines="1  5-10"
{!./docs_src/tutorial/many_to_many/tutorial002_py310.py[ln:1]!}

# 这里省略了一些代码 👈

{!./docs_src/tutorial/many_to_many/tutorial002_py310.py[ln:72-77]!}

# 下方代码被省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3  7-12"
{!./docs_src/tutorial/many_to_many/tutorial002_py39.py[ln:1-3]!}

# 这里省略了一些代码 👈

{!./docs_src/tutorial/many_to_many/tutorial002_py39.py[ln:78-83]!}

# 下方代码被省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3  7-12"
{!./docs_src/tutorial/many_to_many/tutorial002.py[ln:1-3]!}

# 这里省略了一些代码 👈

{!./docs_src/tutorial/many_to_many/tutorial002.py[ln:78-83]!}

# 下方代码被省略 👇
```

////

/// details | 👀 查看完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002.py!}
```

////

///

当然，我们需要将 `update_heroes()` 添加到 `main()` 函数中：

//// tab | Python 3.10+

```Python hl_lines="6"
# 上方代码被省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002_py310.py[ln:94-101]!}
```

////

//// tab | Python 3.9+

```Python hl_lines="6"
# 上方代码被省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002_py39.py[ln:100-107]!}
```

////

//// tab | Python 3.7+

```Python hl_lines="6"
# 上方代码被省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002.py[ln:100-107]!}
```

////

/// details | 👀 查看完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002.py!}
```

////

///

## 添加多对多关系

现在，假设 **Spider-Boy** 觉得 **Z-Force** 团队特别酷，决定加入他们。

我们可以使用同样的 **关系属性**，将 `hero_spider_boy` 添加到 `team_z_force.heroes` 中。

//// tab | Python 3.10+

```Python hl_lines="10-12  14-15"
# 上方代码被省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002_py310.py[ln:72-84]!}

# 下方代码被省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="10-12  14-15"
# 上方代码被省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002_py39.py[ln:78-90]!}

# 下方代码被省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="10-12  14-15"
# 上方代码被省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002.py[ln:78-90]!}

# 下方代码被省略 👇
```

////

/// details | 👀 查看完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002.py!}
```

////

///

/// tip

由于我们在提交后立即访问模型中的属性（如 `hero_spider_boy.teams` 和 `team_z_force.heroes`），数据会自动刷新。

因此，无需调用 `session.refresh()`。

///

然后我们提交更改、刷新并打印更新后的 **Spider-Boy** 的队伍以确认。

注意，我们只将 **Z-Force** 添加到会话中，然后提交。

我们从未将 **Spider-Boy** 添加到会话中，也从未刷新它。但我们仍然打印了他的队伍。

这一切能够正常工作是因为在模型中的 `Relationship()` 使用了 `back_populates`。这样，**SQLModel**（实际上是 SQLAlchemy）可以跟踪更改和更新，并确保这些更新也会发生在其他相关模型的关系中。🎉

## 运行程序

您可以通过在命令行运行程序来确认一切是否正常工作：

<div class="termy">

```console
$ python app.py

// 上面的输出省略 🙈

// 创建新的多对多关系
INFO Engine INSERT INTO heroteamlink (team_id, hero_id) VALUES (?, ?)
INFO Engine [生成于 0.00020s] (1, 3)
INFO Engine COMMIT

// 开始一个新的自动事务
INFO Engine BEGIN (implicit)

// 访问属性 .teams 时自动刷新数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [生成于 0.00044s] (3,)
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team, heroteamlink
WHERE ? = heroteamlink.hero_id AND team.id = heroteamlink.team_id
INFO Engine [缓存自 0.1648s 前] (3,)

// 打印 Spider-Boy 的队伍，包括 Z-Force 🎉
更新后的 Spider-Boy 的队伍: [
    Team(id=2, name='Preventers', headquarters='Sharp Tower'),
    Team(id=1, name='Z-Force', headquarters='Sister Margaret's Bar')
]

// 访问属性 .heroes 时自动刷新数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero, heroteamlink
WHERE ? = heroteamlink.team_id AND hero.id = heroteamlink.hero_id
INFO Engine [缓存自 0.1499s 前] (1,)

// 打印 Z-Force 的英雄，包括 Spider-Boy 🎉
Z-Force 的英雄: [
    Hero(name='Deadpond', age=None, id=1, secret_name='Dive Wilson'),
    Hero(name='Spider-Boy', age=None, id=3, secret_name='Pedro Parqueador', teams=[
        Team(id=2, name='Preventers', headquarters='Sharp Tower'),
        Team(id=1, name='Z-Force', headquarters='Sister Margaret's Bar', heroes=[...])
    ])
]
```

</div>

## 移除多对多关系

现在，假设 **Spider-Boy** 加入团队后，发现他们的“生命保护政策”比他习惯的要宽松得多。💀

而且他们的 *职业安全和健康* 也不如预期... 💥

所以，**Spider-Boy** 决定离开 **Z-Force**。

让我们更新关系，移除 `hero_spider_boy.teams` 中的 `team_z_force`。

由于 `hero_spider_boy.teams` 只是一个列表（实际上是由 SQLAlchemy 管理的特殊列表，但仍然是列表），我们可以使用标准的列表方法。

在这种情况下，我们使用 `.remove()` 方法，它接受一个项目并将其从列表中移除。

//// tab | Python 3.10+

```Python hl_lines="17-19  21-22"
# 上方代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002_py310.py[ln:72-91]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="17-19  21-22"
# 上方代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002_py39.py[ln:78-97]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="17-19  21-22"
# 上方代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial002.py[ln:78-97]!}

# 下方代码省略 👇
```

////

/// details | 👀 查看完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial002.py!}
```

////

///

这次，我们再次展示，使用 `back_populates` 时，**SQLModel**（实际上是 SQLAlchemy）会自动处理通过关系连接模型的工作，即使我们是从 `hero_spider_boy` 对象（修改 `hero_spider_boy.teams`）执行操作，实际上我们是在将 `team_z_force` 添加到 **会话** 中，并提交它，而不需要显式地添加 `hero_spider_boy`。

这依然有效，因为通过更新 `hero_spider_boy` 中的队伍（它们与 `back_populates` 同步），这些更改也会反映到 `team_z_force` 中，因此它也有更改需要保存到数据库中（即 **Spider-Boy** 被移除）。

然后，我们将该团队添加到会话，并提交更改，从而更新 `team_z_force` 对象。由于它更改了与 `hero_spider_boy` 相关联的表，它也会在内部标记为已更新，因此一切都正常工作。

最后，我们再次打印它们，以确认一切是否正常。

## 再次运行程序

为了确认最后的部分是否正常工作，您可以再次运行程序，它会输出类似如下内容：

<div style="font-size: 1rem;" class="termy">

```console
$ python app.py

// 上面的输出省略 🙈

// 删除连接表中的行
INFO Engine DELETE FROM heroteamlink WHERE heroteamlink.team_id = ? AND heroteamlink.hero_id = ?
INFO Engine [生成于 0.00043s] (1, 3)
// 保存更改
INFO Engine COMMIT

// 自动开始一个新的事务
INFO Engine BEGIN (implicit)

// 访问属性 .heroes 时自动刷新数据
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [生成于 0.00029s] (1,)
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero, heroteamlink
WHERE ? = heroteamlink.team_id AND hero.id = heroteamlink.hero_id
INFO Engine [缓存自 0.5625s 前] (1,)

// 打印撤销更改后的 Z-Force 英雄
撤销后的 Z-Force 英雄: [
    Hero(name='Deadpond', age=None, id=1, secret_name='Dive Wilson')
]

// 访问属性 .teams 时自动刷新数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [缓存自 0.4209s 前] (3,)
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team, heroteamlink
WHERE ? = heroteamlink.hero_id AND team.id = heroteamlink.team_id
INFO Engine [缓存自 0.5842s 前] (3,)

// 打印撤销更改后的 Spider-Boy 队伍
撤销后的 Spider-Boy 队伍: [
    Team(id=2, name='Preventers', headquarters='Sharp Tower')
]

// 自动回滚任何可能尚未保存的事务
INFO Engine ROLLBACK

```

</div>

## 总结

在设置好 **连接模型** 和关系属性之后，更新和移除多对多关系是相当简单的。

您只需使用常见的列表操作即可。 🚀
