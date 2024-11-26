# 关系 `back_populates`

现在你已经知道如何使用 **关系属性** 来操作数据库中连接的数据了！🎉

接下来，我们稍微回顾一下之前定义 `Relationship()` 属性的方式，并澄清一下 `back_populates` 参数的含义。🤓

## 带有 `back_populates` 的关系

那么，`Relationship()` 中的 `back_populates` 参数到底是什么呢？

它的值是一个字符串，表示**另一个**模型类中属性的名称。

<img src="/img/tutorial/relationships/attributes/back-populates.svg">

这告诉 **SQLModel**，如果当前模型中发生了变化，它应该同步更新另一个模型中的对应属性，而且即使在提交（commit）之前，它也会立刻生效（而不需要强制刷新数据）。

让我们通过一个例子来更好地理解这一点。

## 不完整的关系

首先，我们通过不使用 `back_populates` 来写一个**不完整**的版本，看看它是如何工作的：

//// tab | Python 3.10+

```Python hl_lines="9  19"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py[ln:1-19]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py[ln:1-21]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py[ln:1-21]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py!}
```

////

///

## 读取数据对象

现在，我们将获取 **Spider-Boy** 英雄，并独立地获取 **Preventers** 队伍，使用两个 `select` 语句。

如你所知，如何执行这两个操作，我就不再分开讲解 `select` 语句、`results` 等内容了。我们直接使用更简洁的形式，通过一个调用来完成：

//// tab | Python 3.10+

```Python hl_lines="5-7  9-11"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py[ln:103-111]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="5-7  9-11"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py[ln:105-113]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5-7  9-11"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py[ln:105-113]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py!}
```

////

///

/// tip

在编写自己的代码时，这可能是你最常用的风格，因为它更加简洁、方便，并且你依然能享受自动补全和内联错误提示的所有优势。

///

## 打印数据

现在，让我们打印当前的 **Spider-Boy**，当前的 **Preventers** 队伍，特别是当前 **Preventers** 英雄列表：

//// tab | Python 3.10+

```Python hl_lines="13-15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py[ln:103-115]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="13-15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py[ln:105-117]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="13-15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py[ln:105-117]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py!}
```

////

///

到目前为止，一切正常。😊

特别是，打印 `preventers_team.heroes` 的结果是：

```hl_lines="3"
Preventers 队伍英雄：[
        Hero(name='Rusty-Man', age=48, id=2, secret_name='Tommy Sharp', team_id=2),
        Hero(name='Spider-Boy', age=None, id=3, secret_name='Pedro Parqueador', team_id=2),
        Hero(name='Tarantula', age=32, id=6, secret_name='Natalia Roman-on', team_id=2),
        Hero(name='Dr. Weird', age=36, id=7, secret_name='Steve Weird', team_id=2),
        Hero(name='Captain North America', age=93, id=8, secret_name='Esteban Rogelios', team_id=2)
]
```

注意，我们在这里看到了 **Spider-Boy** 。

## 提交前更新对象

现在让我们更新 **Spider-Boy**，通过将 `hero_spider_boy.team = None` 来将他从队伍中移除，然后再打印这个对象：

//// tab | Python 3.10+

```Python hl_lines="8  12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py[ln:103-104]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py[ln:117-121]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8  12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py[ln:105-106]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py[ln:119-123]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8  12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py[ln:105-106]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py[ln:119-123]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py!}
```

////

///

首先需要注意的是，我们**还没有提交**英雄对象，因此访问英雄列表不会触发自动刷新。

但在我们的代码中，恰恰在这个时刻，我们已经声明 **Spider-Boy** 不再是 **Preventers** 队伍的一员了。🔥

/// tip

我们以后可以通过不提交 **session** 来撤销这一更改，但这不是我们在这里关注的内容。

///

在此时的代码中，在内存中，代码预期 **Preventers** 不再包括 **Spider-Boy**。

打印没有队伍的 `hero_spider_boy` 的输出是：

```
没有队伍的 Spider-Boy: name='Spider-Boy' age=None id=3 secret_name='Pedro Parqueador' team_id=2 team=None
```

很酷，队伍已经被设置为 `None`，`team_id` 属性仍然保留队伍 ID，直到我们保存它。但没关系，因为我们现在主要是在操作 **关系属性** 和对象。✅

但是现在，当我们打印 `preventers_team.heroes` 时会发生什么？

```hl_lines="3"
Preventers 队伍英雄（更新后）: [
        Hero(name='Rusty-Man', age=48, id=2, secret_name='Tommy Sharp', team_id=2),
        Hero(name='Spider-Boy', age=None, id=3, secret_name='Pedro Parqueador', team_id=2, team=None),
        Hero(name='Tarantula', age=32, id=6, secret_name='Natalia Roman-on', team_id=2),
        Hero(name='Dr. Weird', age=36, id=7, secret_name='Steve Weird', team_id=2),
        Hero(name='Captain North America', age=93, id=8, secret_name='Esteban Rogelios', team_id=2)
]
```

哦，不！😱 **Spider-Boy** 仍然出现在列表中！

## 提交并打印

现在，如果我们提交更改并再次打印：

//// tab | Python 3.10+

```Python hl_lines="8-9  15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py[ln:103-104]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py[ln:123-130]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8-9  15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py[ln:105-106]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py[ln:125-132]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8-9  15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py[ln:105-106]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py[ln:125-132]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial001.py!}
```

////

///

在我们提交后访问 `preventers_team.heroes` 时，会触发数据刷新，所以我们得到了最新的列表， **Spider-Boy** 被移除了，结果就正常了：

```
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age, hero.team_id AS hero_team_id
FROM hero
WHERE ? = hero.team_id
2021-08-13 11:15:24,658 INFO sqlalchemy.engine.Engine [cached since 0.1924s ago] (2,)

Preventers Team Heroes after commit: [
        Hero(name='Rusty-Man', age=48, id=2, secret_name='Tommy Sharp', team_id=2),
        Hero(name='Tarantula', age=32, id=6, secret_name='Natalia Roman-on', team_id=2),
        Hero(name='Dr. Weird', age=36, id=7, secret_name='Steve Weird', team_id=2),
        Hero(name='Captain North America', age=93, id=8, secret_name='Esteban Rogelios', team_id=2)
]
```

提交后没有 **Spider-Boy** ，所以一切正常。😊

不过，在之前的那一部分，我们依然存在不一致的情况。

如果我们在提交前就使用了对象，可能会遇到错误。😔

让我们来修复这个问题。🤓

## 使用 `back_populates` 修复

这就是 `back_populates` 的作用。✨

让我们将其添加回来：

//// tab | Python 3.10+

```Python hl_lines="9  19"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py[ln:1-19]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py[ln:1-21]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py[ln:1-21]!}

# 代码下面部分省略 👇
```

////

/// 详细信息 | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py!}
```

////

///

然后我们可以保持其余代码不变：

//// tab | Python 3.10+

```Python hl_lines="8  12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py[ln:103-104]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py[ln:117-121]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8  12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py[ln:105-106]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py[ln:119-123]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8  12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py[ln:105-106]!}

        # 这里的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py[ln:119-123]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py!}
```

////

///

/// tip

这是我们之前将 `hero_spider_boy.team` 设置为 `None` 但*尚未提交*该更改的同一部分。

也就是之前导致问题的部分。

///

## 回顾结果

这次， **SQLModel** （实际上是 SQLAlchemy）能够注意到更改，并 **自动更新团队中的英雄列表** ，即使我们还没有提交。

第二次打印将输出：

```
Preventers Team Heroes again: [
        Hero(name='Rusty-Man', age=48, id=2, secret_name='Tommy Sharp', team_id=2),
        Hero(name='Tarantula', age=32, id=6, secret_name='Natalia Roman-on', team_id=2),
        Hero(name='Dr. Weird', age=36, id=7, secret_name='Steve Weird', team_id=2),
        Hero(name='Captain North America', age=93, id=8, secret_name='Esteban Rogelios', team_id=2)
]
```

请注意，现在 **Spider-Boy** 不再出现在列表中，我们通过 `back_populates` 修复了这一点！🎉

## `back_populates` 的价值

现在你知道了 `back_populates` 的作用，让我们再次回顾它的具体价值。

代码其实很简单，它只是一个字符串，但可能会让人困惑的是，究竟应该在这里使用什么字符串：

//// tab | Python 3.10+

```Python hl_lines="9  19"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py[ln:1-19]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py[ln:1-21]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11  21"
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py[ln:1-21]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py!}
```

////

///

`back_populates` 中的字符串是指在 **另一个** 模型中，将引用 **当前** 模型的属性。

<img src="../../../img/tutorial/relationships/attributes/back-populates.svg">

因此，在 `Team` 类中，我们有一个属性 `heroes`，并使用 `Relationship(back_populates="team")` 来声明它。

//// tab | Python 3.10+

```Python hl_lines="8"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py[ln:4-9]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="8"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py[ln:6-11]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py[ln:6-11]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py!}
```

////

///

`back_populates="team"` 中的字符串指的是 `Hero` 类（另一个类）中的 `team` 属性。

在 `Hero` 类中，我们声明了一个属性 `team`，并使用 `Relationship(back_populates="heroes")` 来声明它。

因此，字符串 `"heroes"` 指的是 `Team` 类中的 `heroes` 属性。

//// tab | Python 3.10+

```Python hl_lines="10"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py[ln:12-19]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="10"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py[ln:14-21]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="10"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py[ln:14-21]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial002.py!}
```

////

///

/// tip

每个 **关系属性** 都通过 `back_populates` 指向另一个模型中的对应属性。

///

尽管代码很简单，但理解时可能会让人困惑 😵，因为同一行代码涉及到了多个模型的概念：

* 仅仅因为它位于 **当前** 模型中，这行代码与当前模型相关。
* 属性的名称与 **另一个** 模型相关。
* 类型注解与 **另一个** 模型相关。
* `back_populates` 指向的是 **另一个** 模型中的属性，而这个属性指向的是 **当前** 模型。

## 记住 `back_populates` 的一个心理技巧

你可以使用的一个心理技巧是，`back_populates` 中的字符串始终是关于你正在编辑的当前模型类的。🤓

因此，如果你在 `Hero` 类中，`back_populates` 中的任何关系属性，无论它连接到 **任何** 其他表（比如 `Team`、`Weapon`、`Powers` 等），都会始终指向这个相同的类。

所以，`back_populates` 的值很可能是 `"hero"` 或 `"heroes"`。

<img src="../../../img/tutorial/relationships/attributes/back-populates2.svg">

//// tab | Python 3.10+

```Python hl_lines="3  10  13  15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial003_py310.py[ln:27-39]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3  10  13  15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial003_py39.py[ln:29-41]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3  10  13  15"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial003.py[ln:29-41]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial003_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial003_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/back_populates/tutorial003.py!}
```

////

///
