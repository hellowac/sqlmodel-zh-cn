# 读取单行数据

你已经知道如何使用 `.where()` 来筛选行进行选择。

你也看到过，当执行 `select()` 时，它通常返回一个 **可迭代** 对象。

或者你可以调用 `results.all()` 来立即获取所有行的 **列表** ，而不是可迭代对象。

但是在许多情况下，你只想读取 **单行数据** ，而处理可迭代对象或列表并不那么方便。

接下来，我们来看一下读取单行数据的实用工具。

## 从上一个代码继续

我们将继续使用前几章中用来创建和选择数据的相同示例，并继续更新它们。

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/indexes/tutorial002_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/indexes/tutorial002.py!}
```

////

///

如果你已经执行了之前的示例并且有一个数据库文件， **在运行每个示例之前删除数据库文件** ，这样你就不会有重复数据，并且可以获得相同的结果。

## 读取第一行

我们一直在遍历 `result` 对象中的行，像这样：

//// tab | Python 3.10+

```Python hl_lines="7-8"
# Code above omitted 👆

{!./docs_src/tutorial/indexes/tutorial002_py310.py[ln:42-47]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="7-8"
# Code above omitted 👆

{!./docs_src/tutorial/indexes/tutorial002.py[ln:44-49]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/indexes/tutorial002_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/indexes/tutorial002.py!}
```

////

///

但假设我们并不关心所有的行，只关心 **第一行**。

我们可以在 `results` 对象上调用 `.first()` 方法来获取第一行：

//// tab | Python 3.10+

```Python hl_lines="7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial001_py310.py[ln:42-47]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial001.py[ln:44-49]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial001.py!}
```

////

///

这将返回 `results` 中的第一行对象（如果有的话）。

这样，我们就不需要处理可迭代对象或列表了。

/// tip

请注意，`.first()` 是 `results` 对象的方法，而不是 `select()` 语句的方法。

///

虽然这个查询会找到两行数据，但通过使用 `.first()` 我们只会获得第一行。

如果我们在命令行中运行它，输出将是：

<div class="termy">

```console
$ python app.py

// 一些初始化输出被省略 😉

// 带有 WHERE 的 SELECT 查询
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.age <= ?
INFO Engine [no key 0.00021s] (35,)

// 只打印第一项
Hero: secret_name='Natalia Roman-on' age=32 id=4 name='Tarantula'
```

</div>

## 第一个或 `None`

SQL 查询可能不会找到任何行。

在这种情况下，`.first()` 将返回 `None`：

//// tab | Python 3.10+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial002_py310.py[ln:42-47]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial002.py[ln:44-49]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial002_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial002.py!}
```

////

///

在这种情况下，由于没有年龄小于 25 的英雄，`.first()` 将返回 `None`。

当我们在命令行运行它时，输出将是：

<div class="termy">

```console
$ python app.py

// 一些初始化输出被省略 😉

// 带有 WHERE 的 SELECT 查询
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.age <= ?
INFO Engine [no key 0.00021s] (35,)

// 没有找到行，第一行是 None
Hero: None
```

</div>

## 精确匹配一行

有时我们希望确保查询结果中只有 **一行** 数据。

如果查询结果中多于一行，则意味着系统出现了错误，我们应该终止并抛出一个错误。

在这种情况下，我们可以使用 `.one()`，而不是 `.first()`：

//// tab | Python 3.10+

```Python hl_lines="7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial003_py310.py[ln:42-47]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial003.py[ln:44-49]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial003_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial003.py!}
```

////

///

在这里，我们知道 `"Deadpond"` 只有一行，而且不应该有多于一行的记录。

如果我们运行一次，输出将会是：

<div class="termy">

```console
$ python app.py

// 一些初始化输出被省略 😉

// 带有 WHERE 的 SELECT 查询
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.name = ?
INFO Engine [no key 0.00015s] ('Deadpond',)

// 只找到了一行，没问题 ✅
Hero: secret_name='Dive Wilson' age=None id=1 name='Deadpond'
```

</div>

但如果我们再次运行，由于它会重新创建并插入所有英雄记录到数据库中，这会导致重复的记录，并且会有多个 `"Deadpond"`。 😱

因此，如果我们没有先删除 `database.db` 文件，直接运行它，将会输出：

<div class="termy">

```console
$ python app.py

// 一些初始化输出被省略 😉

// 带有 WHERE 的 SELECT 查询
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.name = ?
INFO Engine [no key 0.00015s] ('Deadpond',)

// 哎呀，数据库处于损坏状态，有重复记录！ 🚨
Traceback (most recent call last):

// 错误的详细信息省略

sqlalchemy.exc.MultipleResultsFound: Multiple rows were found when exactly one was required
```

</div>

## 精确匹配一行（有更多数据时）

当然，即使我们没有重复数据，如果发送一个查询返回多于一行的结果，并且仍然期望 `.one()` 返回正好一行，我们也会遇到相同的错误：

//// tab | Python 3.10+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial004_py310.py[ln:42-47]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial004.py[ln:44-49]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial004_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial004.py!}
```

////

///

这个查询将会找到 2 行数据，最终也会抛出相同的错误。

## 精确匹配一行（没有数据时）

如果我们使用 `.one()` 但查询没有返回任何行，它也会抛出一个错误：

//// tab | Python 3.10+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial005_py310.py[ln:42-47]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial005.py[ln:44-49]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial005_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial005.py!}
```

////

///

在这种情况下，由于没有年龄小于 25 的英雄，`.one()` 会抛出一个错误。

如果我们在命令行运行它，输出会是这样的：

<div class="termy">

```console
$ python app.py

// Some boilerplate output omitted 😉

// SELECT with WHERE
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.age < ?
INFO Engine [no key 0.00014s] (25,)

// 哎呀，我们期望有一行数据，但没有找到任何！🚨
Traceback (most recent call last):

// 一些错误的详细信息被省略

sqlalchemy.exc.NoResultFound: No row was found when one was required
```

</div>

## 简洁版本

当然，使用 `.first()` 和 `.one()` 时，通常你会将代码写得更加简洁，大多数情况下可以将其写成一行（或者至少是一条 Python 语句）：

//// tab | Python 3.10+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial006_py310.py[ln:42-45]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial006.py[ln:44-47]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial006_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial006.py!}
```

////

///

这将与上面的例子得到相同的结果。

## 使用 `.where()` 根据 Id 进行选择

在许多情况下，你可能想通过 **主键** 的 Id 列选择单行数据。

你可以像我们之前做的那样，使用 `.where()` 然后用 `.first()` 获取第一项：

//// tab | Python 3.10+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial007_py310.py[ln:42-47]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5  7"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial007.py[ln:44-49]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial007_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial007.py!}
```

////

///

这将按预期正常工作。但其实有一个更简洁的版本。👇

## 使用 `.get()` 根据 Id 进行选择

由于根据 **主键** 的 Id 列选择单行数据是一个常见操作，因此有一个简便的方法来实现：

//// tab | Python 3.10+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial008_py310.py[ln:42-45]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial008.py[ln:44-47]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial008_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial008.py!}
```

////

///

`session.get(Hero, 1)` 等效于创建一个 `select()` 查询，使用 `.where()` 根据 Id 进行筛选，然后使用 `.first()` 获取第一项。

如果你运行它，输出将是：

<div class="termy">

```console
$ python app.py

// 一些基本输出被省略 😉

// SELECT with WHERE
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00021s] (1,)

// 打印结果
Hero: secret_name='Dive Wilson' age=None id=1 name='Deadpond'
```

</div>

## 使用 `.get()` 根据 Id 进行选择（无数据）

`.get()` 的行为类似于 `.first()`，如果没有数据，它将返回 `None`（而不是抛出错误）：

//// tab | Python 3.10+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial009_py310.py[ln:42-45]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5"
# Code above omitted 👆

{!./docs_src/tutorial/one/tutorial009.py[ln:44-47]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/one/tutorial009_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/one/tutorial009.py!}
```

////

///

运行这段代码将输出：

<div class="termy">

```console
$ python app.py

// 一些基本输出被省略 😉

// SELECT with WHERE
INFO Engine BEGIN (implicit)
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age
FROM hero
WHERE hero.id = ?
INFO Engine [generated in 0.00024s] (9001,)

// 没有找到数据，因此值为 None
Hero: None
```

</div>

## 总结

由于查询 SQL 数据库中的单行数据是一个常见操作，现在你有了几种工具，可以以简洁的方式实现这一功能。🎉
