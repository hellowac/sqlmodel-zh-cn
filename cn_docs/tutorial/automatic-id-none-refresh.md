# 自动生成的 ID、`None` 默认值和数据刷新

在上一章中，我们学习了如何使用 **SQLModel** 向数据库添加行。

现在，让我们详细探讨一下为什么 `id` 字段在数据库中作为 **主键** 时 **不能为 `NULL`**，这是因为我们通过 `Field(primary_key=True)` 声明它。

然而，在 Python 代码中，同一个 `id` 字段实际上 **可以是 `None`**。因此，我们使用 `int | None`（或 `Optional[int]`）声明其类型，并设置默认值为 `Field(default=None)`：

//// tab | Python 3.10+

```Python hl_lines="4"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:4-8]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="4"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:6-10]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

接下来，我将详细介绍数据库与 Python 代码之间的数据同步。

比如，什么时候会从数据库中获取 `id` 字段的实际 `int` 值？让我们一起来探讨这些内容。👇

## 创建一个新的 `Hero` 实例

当我们创建一个新的 `Hero` 实例时，我们并没有设置 `id`：

//// tab | Python 3.10+

```Python hl_lines="3-6"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:21-24]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-6"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:23-26]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

### `Optional` 如何帮助

由于我们没有设置 `id`，它会采用我们在 `Field(default=None)` 中设置的 Python 默认值 `None`。

这就是为什么我们使用 `Optional` 并将默认值设置为 `None` 的唯一原因。

因为在代码的这一部分，**在与数据库交互之前**，Python 的值实际上可能是 `None`。

如果我们假设 `id` 总是一个 `int`，并且没有使用 `Optional` 添加类型注解，我们可能会写出错误的代码，例如：

```Python
next_hero_id = hero_1.id + 1
```

如果我们在将英雄保存到数据库之前运行这段代码，并且 `hero_1.id` 仍然是 `None`，我们会得到如下错误：

```
TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```

但是通过使用 `Optional[int]` 声明，编辑器会帮助我们避免写出错误的代码，并提醒我们如果 `hero_1.id` 是 `None`，代码可能无效。🔍

## 打印默认的 `id` 值

我们可以通过在将英雄添加到数据库之前打印它们来确认这一点：

//// tab | Python 3.10+

```Python hl_lines="9-11"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:21-29]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9-11"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:23-31]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

输出将是：

<div class="termy">

```console
$ python app.py

// 上方输出省略 👆

在与数据库交互之前
Hero 1: id=None name='Deadpond' secret_name='Dive Wilson' age=None
Hero 2: id=None name='Spider-Boy' secret_name='Pedro Parqueador' age=None
Hero 3: id=None name='Rusty-Man' secret_name='Tommy Sharp' age=48
```

</div>

注意它们的 `id=None`。

这是我们在 `Hero` 模型类中定义的默认值。

当我们将这些对象 `add` 到 **session** 时会发生什么？

## 将对象添加到 Session

在我们将 `Hero` 实例对象添加到 **session** 后，ID 仍然是 `None`。

我们可以通过使用 `with` 块创建一个 session，添加对象，然后再次打印它们来验证这一点：

//// tab | Python 3.10+

```Python hl_lines="19-21"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:21-39]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="19-21"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:23-41]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

这将再次输出对象的 `id` 为 `None`：

<div class="termy">

```console
$ python app.py

// 上方输出省略 👆

添加到 session 后
Hero 1: id=None name='Deadpond' secret_name='Dive Wilson' age=None
Hero 2: id=None name='Spider-Boy' secret_name='Pedro Parqueador' age=None
Hero 3: id=None name='Rusty-Man' secret_name='Tommy Sharp' age=48
```

</div>

正如我们之前所见，**session** 很智能，不会在每次准备更改时都与数据库通信，只有在我们准备好并告诉它 `commit` 更改时，它才会将所有 SQL 发送到数据库以存储数据。

## 提交更改到数据库

然后我们可以提交 session 中的更改，并再次打印：

//// tab | Python 3.10+

```Python hl_lines="13 16-18"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:31-46]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="13 16-18"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:33-48]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

现在，发生了一些意想不到的事情，看看输出，似乎 `Hero` 实例对象根本没有数据：

<div class="termy">

```console
$ python app.py

// 上方输出省略 👆

// 这里是引擎与数据库交互的部分，SQL 操作
INFO Engine BEGIN (implicit)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [generated in 0.00018s] ('Deadpond', 'Dive Wilson', None)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.0008968s ago] ('Spider-Boy', 'Pedro Parqueador', None)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.001143s ago] ('Rusty-Man', 'Tommy Sharp', 48)
INFO Engine COMMIT

// 现在是我们的打印输出
提交 session 后
Hero 1:
Hero 2:
Hero 3:

// 这里发生了什么？😱
```

</div>

发生的情况是 SQLModel（实际上是 SQLAlchemy）内部将这些对象标记为“过期”，它们 **没有最新版本的数据** 。这是因为我们可能在数据库中更新了一些字段，例如，假设有一个字段 `updated_at: datetime`，它在我们保存更改时会自动更新。

同样，其他值也可能发生了变化，因此 **session** 为了确保安全，会内部将对象标记为过期。

然后，下次我们访问每个属性时，例如：

```Python
current_hero_name = hero_1.name
```

...SQLModel（实际上是 SQLAlchemy）会确保联系数据库并 **获取数据的最新版本** ，更新对象中的 `name` 字段，然后使其对后续的 Python 表达式可用。在上面的例子中，Python 会继续执行，并使用刚刚更新的 `hero_1.name` 值将其赋值给变量 `current_hero_name`。

这一切都会自动发生，并且是幕后进行的。✨

而且这是我们示例中的有趣和奇怪之处：

```Python
print("Hero 1:", hero_1)
```

我们并没有访问对象的属性，如 `hero.name`。我们只是访问了整个对象并打印了它，所以 **SQLAlchemy 无法知道** 我们想访问该对象的数据。

## 打印单个字段

为了确认和理解当访问属性时如何进行**自动过期和数据刷新**，我们可以打印一些单独的字段（实例属性）：

//// tab | Python 3.10+

```Python hl_lines="21-23 26-28"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:31-56]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="21-23 26-28"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:33-58]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

现在我们实际上正在访问属性，因为我们不再打印整个对象 `hero_1`：

```Python
print("Hero 1:", hero_1)
```

...我们现在打印的是 `hero.id` 中的 `id` 属性：

```Python
print("Hero 1 ID:", hero_1.id)
```

通过访问属性，这**触发**了 SQLModel（实际上是 SQLAlchemy）在后台进行大量工作，**从数据库刷新数据**，将其设置到对象的 `id` 属性中，并使其在 Python 表达式中可用（在本例中就是打印出来）。

让我们看看它是如何工作的：

<div class="termy">

```console
$ python app.py

// 上方输出省略 👆

// 提交后，对象已过期且没有值
提交 session 后
Hero 1:
Hero 2:
Hero 3:

// 现在我们将访问像 ID 这样的属性，这是第一次打印
提交 session 后，显示 ID

// 请注意，在打印第一个 ID 之前，Session 会让 Engine 去数据库刷新数据 🤓
INFO Engine BEGIN (implicit)
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00017s] (1,)

// 这是我们的第一次打印，现在我们有了数据库生成的 ID
Hero 1 ID: 1

// 在打印下一个 ID 之前，刷新第二个对象的数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.001245s ago] (2,)

// 这是我们的第二个英雄打印，带有自动生成的 ID
Hero 2 ID: 2

// 在第三个打印之前，刷新它的数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.002215s ago] (3,)

// 这是我们的第三个英雄打印
Hero 3 ID: 3

// 如果我们打印另一个属性，比如名字呢？
提交 session 后，显示名字
Hero 1 name: Deadpond
Hero 2 name: Spider-Boy
Hero 3 name: Rusty-Man

// 因为 Session 已经刷新了这些对象的所有数据，并且 Session 知道它们没有过期，所以它不需要再次去数据库获取名字 🤓
```

</div>

## 显式刷新对象

你刚刚学习了如何通过 **session** 在后台自动刷新数据，当你访问某个属性时，数据会作为副作用被刷新。

但是如果你想 **显式刷新** 数据怎么办呢？

你也可以使用 `session.refresh(object)` 来做到这一点：

//// tab | Python 3.10+

```Python hl_lines="30-32 35-37"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:31-65]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="30-32 35-37"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:33-67]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

当 Python 执行以下代码时：

```Python
session.refresh(hero_1)
```

... **session** 会让 **engine** 与数据库进行通信，获取该对象 `hero_1` 的最新数据，然后将数据放入 `hero_1` 对象中，并将其标记为“新鲜”或“未过期”。

以下是输出的样子：

<div class="termy">

```console
$ python app.py

// 上方输出省略 👆

// 第一次刷新
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00024s] (1,)

// 第二次刷新
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.001487s ago] (2,)

// 第三次刷新
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.002377s ago] (3,)

// 现在打印数据，由于数据已经刷新，Session 不需要再次刷新
刷新英雄后
Hero 1: age=None id=1 name='Deadpond' secret_name='Dive Wilson'
Hero 2: age=None id=2 name='Spider-Boy' secret_name='Pedro Parqueador'
Hero 3: age=48 id=3 name='Rusty-Man' secret_name='Tommy Sharp'
```

</div>

这在某些情况下会很有用，例如，如果你正在构建一个用于创建英雄的 Web API。假设一旦创建了某个英雄并保存了一些数据，你将其返回给客户端。

你不希望返回一个看起来空空的对象，因为没有触发自动刷新数据的机制。

在这种情况下，提交对象到数据库后，你可以显式刷新它，然后将其返回给客户端。这将确保对象包含最新的数据。

## 关闭会话后打印数据

现在，作为最后一个实验，我们还可以在 **会话** 关闭后打印数据。

这里没有什么意外，依然能够正常工作：

//// tab | Python 3.10+

```Python hl_lines="40-42"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py[ln:31-70]!}

# 下方代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="40-42"
# 上方代码省略 👆

{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py[ln:33-72]!}

# 下方代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial001.py!}
```

////

///

输出再次显示相同的数据：

<div class="termy">

```console
$ python app.py

// 上方输出省略 👆

// 在完成 with 块后，会话已关闭，包括任何未提交的挂起事务的回滚
INFO Engine ROLLBACK

// 然后我们打印数据，正常工作
会话关闭后
Hero 1: age=None id=1 name='Deadpond' secret_name='Dive Wilson'
Hero 2: age=None id=2 name='Spider-Boy' secret_name='Pedro Parqueador'
Hero 3: age=48 id=3 name='Rusty-Man' secret_name='Tommy Sharp'
```

</div>

## 回顾所有代码

现在让我们再次回顾所有的代码。

/// tip

每个编号气泡都会显示该行代码在输出中的打印内容。

由于我们在创建**引擎**时使用了 `echo=True`，我们可以看到在每个步骤中执行的 SQL 语句。

///

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial002_py310.py!}
```

{!./docs_src/tutorial/automatic_id_none_refresh/annotations/en/tutorial002.md!}

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/automatic_id_none_refresh/tutorial002.py!}
```

{!./docs_src/tutorial/automatic_id_none_refresh/annotations/en/tutorial002.md!}

////

以下是运行该程序时生成的所有输出：

<div class="termy">

```console
$ python app.py

INFO Engine BEGIN (implicit)
INFO Engine PRAGMA main.table_info("hero")
INFO Engine [raw sql] ()
INFO Engine PRAGMA temp.table_info("hero")
INFO Engine [raw sql] ()
INFO Engine
CREATE TABLE hero (
        id INTEGER,
        name VARCHAR NOT NULL,
        secret_name VARCHAR NOT NULL,
        age INTEGER,
        PRIMARY KEY (id)
)


INFO Engine [no key 0.00018s] ()
INFO Engine COMMIT
Before interacting with the database
Hero 1: id=None name='Deadpond' secret_name='Dive Wilson' age=None
Hero 2: id=None name='Spider-Boy' secret_name='Pedro Parqueador' age=None
Hero 3: id=None name='Rusty-Man' secret_name='Tommy Sharp' age=48
After adding to the session
Hero 1: id=None name='Deadpond' secret_name='Dive Wilson' age=None
Hero 2: id=None name='Spider-Boy' secret_name='Pedro Parqueador' age=None
Hero 3: id=None name='Rusty-Man' secret_name='Tommy Sharp' age=48
INFO Engine BEGIN (implicit)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [generated in 0.00022s] ('Deadpond', 'Dive Wilson', None)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.001127s ago] ('Spider-Boy', 'Pedro Parqueador', None)
INFO Engine INSERT INTO hero (name, secret_name, age) VALUES (?, ?, ?)
INFO Engine [cached since 0.001483s ago] ('Rusty-Man', 'Tommy Sharp', 48)
INFO Engine COMMIT
After committing the session
Hero 1:
Hero 2:
Hero 3:
After committing the session, show IDs
INFO Engine BEGIN (implicit)
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00029s] (1,)
Hero 1 ID: 1
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.002132s ago] (2,)
Hero 2 ID: 2
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.003367s ago] (3,)
Hero 3 ID: 3
After committing the session, show names
Hero 1 name: Deadpond
Hero 2 name: Spider-Boy
Hero 3 name: Rusty-Man
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00025s] (1,)
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.001583s ago] (2,)
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.002722s ago] (3,)
After refreshing the heroes
Hero 1: age=None id=1 name='Deadpond' secret_name='Dive Wilson'
Hero 2: age=None id=2 name='Spider-Boy' secret_name='Pedro Parqueador'
Hero 3: age=48 id=3 name='Rusty-Man' secret_name='Tommy Sharp'
INFO Engine ROLLBACK
After the session closes
Hero 1: age=None id=1 name='Deadpond' secret_name='Dive Wilson'
Hero 2: age=None id=2 name='Spider-Boy' secret_name='Pedro Parqueador'
Hero 3: age=48 id=3 name='Rusty-Man' secret_name='Tommy Sharp'
```

</div>

## 回顾

你读完了所有这些内容！真是很多啊！来块蛋糕奖励一下自己吧，你值得拥有。🍰

我们讨论了 **会话** 如何使用 **引擎** 向数据库发送 SQL，用于创建数据和获取数据。它如何追踪“ **过期** ”和“ **新鲜** ”的数据。在何时会 **自动获取数据** （访问实例属性时）以及如何通过 **会话** 在内存中的对象和数据库之间同步数据。

如果你理解了这些内容，那么现在你对 **SQLModel** 、SQLAlchemy，以及 Python 与数据库交互的工作原理有了很深入的了解。

如果你没有完全理解也没关系，随时可以回来 **`刷新`** 一下这些概念。<abbr title="看懂我说的了吗？😜">`refresh`</abbr>

我认为这可能是导致问题和让你头疼的主要错误类型之一。所以，做得好，继续努力学习！💪