# 带有额外字段的连接模型

在之前的示例中，我们从未直接与 `HeroTeamLink` 模型进行交互，一切都是通过自动的 **多对多** 关系来完成的。

但如果我们需要额外的数据来描述两个模型之间的连接呢？

假设我们想要有一个额外的字段/列，用来表示一个英雄是否仍然在该队伍中 **训练**，或者他们是否已经开始执行任务等。

让我们看看如何实现这一点。

## 带有两个一对多关系的连接模型

处理这种情况的方式是显式地使用连接模型，以便能够获取和修改其数据（除了指向 `Hero` 和 `Team` 两个模型的外键）。

最终，它的工作方式就像是两个 **一对多** 关系的结合。

在 `heroteamlink` 表中的一行指向 **一个** 特定的英雄，但一个英雄可以与 **多个** 英雄-队伍连接，所以是 **一对多**。

同样，`heroteamlink` 表中的同一行也指向 **一个** 队伍，但一个队伍可以与 **多个** 英雄-队伍连接，所以也是 **一对多**。

/// tip

之前的多对多关系实际上也只是两个一对多关系的结合，但现在它会变得更加显式。

///

## 更新连接模型

我们将更新 `HeroTeamLink` 模型。

我们将添加一个新字段 `is_training`。

同时，我们还会为链接的 `team` 和 `hero` 添加两个 **关系属性**：

//// tab | Python 3.10+

```Python hl_lines="6  8-9"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py310.py[ln:4-10]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="10  12-13"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py39.py[ln:6-16]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="10  12-13"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003.py[ln:6-16]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003.py!}
```

////

///

新的 **关系属性** 有它们自己的 `back_populates`，指向我们将在 `Hero` 和 `Team` 模型中创建的新关系属性：

* `team`：具有 `back_populates="hero_links"`，因为在 `Team` 模型中，该属性将包含指向 **队伍英雄** 的链接。
* `hero`：具有 `back_populates="team_links"`，因为在 `Hero` 模型中，该属性将包含指向 **英雄队伍** 的链接。

/// info

在 SQLAlchemy 中，这被称为关联对象或关联模型（Association Object 或 Association Model）。

我称其为 **连接模型**，仅仅是因为这样写更简单，避免了拼写错误。但您也可以根据需要随意命名。😉

///

## 更新 Team 模型

现在让我们更新 `Team` 模型。

我们不再使用 `heroes` 关系属性，取而代之的是新的 `hero_links` 属性：

//// tab | Python 3.10+

```Python hl_lines="8"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py310.py[ln:13-18]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py39.py[ln:19-24]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003.py[ln:19-24]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003.py!}
```

////

## 更新 Hero 模型

同样的，更新 `Hero` 模型。

我们将 `teams` 关系属性改为 `team_links`：

//// tab | Python 3.10+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py310.py[ln:21-27]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py39.py[ln:27-33]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003.py[ln:27-33]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003.py!}
```

////

///

## 创建关系

现在创建关系的过程非常相似。

但现在我们需要手动创建**显式的链接模型**，这些模型指向它们的英雄和团队实例，并指定额外的链接数据（`is_training`）：

//// tab | Python 3.10+

```Python hl_lines="21-30  32-35"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py310.py[ln:40-79]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="21-30  32-35"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py39.py[ln:46-85]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="21-30  32-35"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003.py[ln:46-85]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003.py!}
```

////

///

我们只需要将链接模型实例添加到会话中，因为链接模型实例已经连接到英雄和团队，当我们提交时，它们也会自动包含在会话中。

## 运行程序

现在，如果我们运行程序，它将显示几乎与之前相同的输出，因为它生成的 SQL 几乎相同，但这次包括了新的 `is_training` 列：

<div class="termy">

```console
$ python app.py

// 省略之前的输出 🙈

// 自动开始一个新事务
INFO Engine BEGIN (implicit)

// 插入英雄数据
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [generated in 0.00025s] ('Deadpond', 'Dive Wilson', None)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.00136s ago] ('Spider-Boy', 'Pedro Parqueador', None)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.001858s ago] ('Rusty-Man', 'Tommy Sharp', 48)

// 插入团队数据
INFO Engine INSERT INTO team (name, headquarters) VALUES (?, ?)
INFO Engine [generated in 0.00019s] ('Z-Force', 'Sister Margaret's Bar')
INFO Engine INSERT INTO team (name, headquarters) VALUES (?, ?)
INFO Engine [cached since 0.0007985s ago] ('Preventers', 'Sharp Tower')

// 插入英雄-团队链接数据
INFO Engine INSERT INTO heroteamlink (team_id, hero_id, is_training) VALUES (?, ?, ?)
INFO Engine [generated in 0.00023s] ((1, 1, 0), (2, 1, 1), (2, 2, 1), (2, 3, 0))
// 在事务中保存更改到数据库
INFO Engine COMMIT

// 自动开始一个新事务
INFO Engine BEGIN (implicit)

// 自动获取属性访问时的数据
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [generated in 0.00028s] (1,)
INFO Engine SELECT heroteamlink.team_id AS heroteamlink_team_id, heroteamlink.hero_id AS heroteamlink_hero_id, heroteamlink.is_training AS heroteamlink_is_training
FROM heroteamlink
WHERE ? = heroteamlink.team_id
INFO Engine [generated in 0.00026s] (1,)
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00024s] (1,)

// 打印 Z-Force 英雄数据，包括链接数据
Z-Force hero: name='Deadpond' age=None id=1 secret_name='Dive Wilson' is training: False

// 自动获取属性访问时的数据
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [cached since 0.008822s ago] (2,)
INFO Engine SELECT heroteamlink.team_id AS heroteamlink_team_id, heroteamlink.hero_id AS heroteamlink_hero_id, heroteamlink.is_training AS heroteamlink_is_training
FROM heroteamlink
WHERE ? = heroteamlink.team_id
INFO Engine [cached since 0.005778s ago] (2,)

// 打印 Preventers 英雄数据，包括链接数据
Preventers hero: name='Deadpond' age=None id=1 secret_name='Dive Wilson' is training: True

// 自动获取属性访问时的数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.004196s ago] (2,)

// 打印 Preventers 英雄数据，包括链接数据
Preventers hero: name='Spider-Boy' age=None id=2 secret_name='Pedro Parqueador' is training: True

// 自动获取属性访问时的数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.006005s ago] (3,)

// 打印 Preventers 英雄数据，包括链接数据
Preventers hero: name='Rusty-Man' age=48 id=3 secret_name='Tommy Sharp' is training: False
```

</div>

## 添加关系

现在，要添加一个新的关系，我们需要创建一个新的 `HeroTeamLink` 实例，指向英雄和团队，添加到会话中，然后提交它。

这里我们在 `update_heroes()` 函数中执行此操作：

//// tab | Python 3.10+

```Python hl_lines="10-15"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py310.py[ln:82-97]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="10-15"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py39.py[ln:88-103]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="10-15"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003.py[ln:88-103]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003.py!}
```

////

///

## 使用新关系运行程序

如果我们运行该程序，将看到如下输出：

<div class="termy">

```console
$ python app.py

// 省略之前的输出 🙈

// 自动开始一个新事务
INFO Engine BEGIN (implicit)

// 选择英雄
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.name = ?
INFO Engine [no key 0.00014s] ('Spider-Boy',)

// 选择团队
INFO Engine SELECT team.id, team.name, team.headquarters
FROM team
WHERE team.name = ?
INFO Engine [no key 0.00012s] ('Z-Force',)

// 创建链接
INFO Engine INSERT INTO heroteamlink (team_id, hero_id, is_training) VALUES (?, ?, ?)
INFO Engine [generated in 0.00023s] (1, 2, 1)

// 自动刷新数据，访问属性时
INFO Engine SELECT heroteamlink.team_id AS heroteamlink_team_id, heroteamlink.hero_id AS heroteamlink_hero_id, heroteamlink.is_training AS heroteamlink_is_training
FROM heroteamlink
WHERE ? = heroteamlink.team_id
INFO Engine [cached since 0.01514s ago] (1,)
INFO Engine COMMIT
INFO Engine BEGIN (implicit)
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.08953s ago] (2,)
INFO Engine SELECT heroteamlink.team_id AS heroteamlink_team_id, heroteamlink.hero_id AS heroteamlink_hero_id, heroteamlink.is_training AS heroteamlink_is_training
FROM heroteamlink
WHERE ? = heroteamlink.hero_id
INFO Engine [generated in 0.00018s] (2,)

// 打印更新后的英雄链接
Updated Spider-Boy's Teams: [
    HeroTeamLink(team_id=2, is_training=True, hero_id=2),
    HeroTeamLink(team_id=1, is_training=True, hero_id=2)
]

// 自动刷新团队数据，访问属性时
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [cached since 0.1084s ago] (1,)
INFO Engine SELECT heroteamlink.team_id AS heroteamlink_team_id, heroteamlink.hero_id AS heroteamlink_hero_id, heroteamlink.is_training AS heroteamlink_is_training
FROM heroteamlink
WHERE ? = heroteamlink.team_id
INFO Engine [cached since 0.1054s ago] (1,)

// 打印团队英雄链接
Z-Force heroes: [
    HeroTeamLink(team_id=1, is_training=False, hero_id=1),
    HeroTeamLink(team_id=1, is_training=True, hero_id=2)
]
```

</div>

## 使用链接更新关系

现在假设 **Spider-Boy** 在 **Preventers** 训练得足够久，团队同意他可以全职加入。

因此，我们现在希望将 `is_training` 的状态更新为 `False`。

我们可以通过迭代链接来实现这一点：

//// tab | Python 3.10+

```Python hl_lines="8-10"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py310.py[ln:82-83]!}

        # 代码省略 👈

{!./docs_src/tutorial/many_to_many/tutorial003_py310.py[ln:99-107]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8-10"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003_py39.py[ln:88-89]!}

        # 代码省略 👈

{!./docs_src/tutorial/many_to_many/tutorial003_py39.py[ln:105-113]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8-10"
# 代码省略 👆

{!./docs_src/tutorial/many_to_many/tutorial003.py[ln:88-89]!}

        # 代码省略 👈

{!./docs_src/tutorial/many_to_many/tutorial003.py[ln:105-113]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/many_to_many/tutorial003.py!}
```

////

///

## 使用更新后的关系运行程序

如果我们现在运行程序，它将输出：

<div class="termy">

```console
$ python app.py

// 省略之前的输出 🙈

// 自动通过属性访问获取团队数据
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [generated in 0.00015s] (2,)

// 更新链接行
INFO Engine UPDATE heroteamlink SET is_training=? WHERE heroteamlink.team_id = ? AND heroteamlink.hero_id = ?
INFO Engine [generated in 0.00020s] (0, 2, 2)

// 保存当前事务到数据库
INFO Engine COMMIT

// 自动开始一个新事务
INFO Engine BEGIN (implicit)

// 自动通过属性访问获取数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.2004s ago] (2,)
INFO Engine SELECT heroteamlink.team_id AS heroteamlink_team_id, heroteamlink.hero_id AS heroteamlink_hero_id, heroteamlink.is_training AS heroteamlink_is_training
FROM heroteamlink
WHERE ? = heroteamlink.hero_id
INFO Engine [cached since 0.1005s ago] (2,)
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [cached since 0.09707s ago] (2,)

// 打印 Spider-Boy 的团队信息，包括链接数据，检查是否在训练
Spider-Boy team: headquarters='Sharp Tower' id=2 name='Preventers' is training: False

// 自动通过属性访问获取数据
INFO Engine SELECT team.id AS team_id, team.name AS team_name, team.headquarters AS team_headquarters
FROM team
WHERE team.id = ?
INFO Engine [cached since 0.2097s ago] (1,)

// 打印 Spider-Boy 的团队信息，包括链接数据，检查是否在训练
Spider-Boy team: headquarters='Sister Margaret's Bar' id=1 name='Z-Force' is training: True
INFO Engine ROLLBACK
```

</div>

## 总结

如果您需要存储更多关于 **多对多** 关系的信息，您可以使用一个显式的链接模型，并在其中包含额外的数据。🤓
