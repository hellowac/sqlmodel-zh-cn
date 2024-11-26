# 删除数据 - DELETE

现在让我们使用 **SQLModel** 删除一些数据。

## 从之前的代码继续

和之前一样，我们将从上次的代码继续。

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/update/tutorial003_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/update/tutorial003.py!}
```

////

///

记得在运行示例之前删除 `database.db` 文件，以获得相同的结果。

## 使用 SQL 删除

这个 `Spider-Youngster` 实在是太奇怪了，我们就把它删除吧。

但不用担心，我们稍后会用一个新的故事重新开始。😅

让我们看看如何使用 **SQL** 删除它：

```SQL hl_lines="1"
DELETE
FROM hero
WHERE name = "Spider-Youngster"
```

这大致的意思是：

> 嘿 SQL 数据库 👋，我想要 `DELETE` 从名为 `hero` 的表中删除行。
>
> 请删除所有 `WHERE` 列 `name` 的值等于 `"Spider-Youngster"` 的行。

记住，当使用 `SELECT` 语句时，它的格式是：

```SQL
SELECT [这里填写一些内容]
FROM [这里填写表名]
WHERE [这里填写条件]
```

`DELETE` 很相似，同样我们使用 `FROM` 来指定操作的表，并用 `WHERE` 来指定匹配我们想删除的行的条件。

你可以在 **DB Browser for SQLite** 中尝试这个操作：

<img class="shadow" src="/img/tutorial/delete/image01.png">

请记住，`DELETE` 是删除整行数据，而不是单一列中的某个值。

如果你想要“删除”列中的某个单一值，但 **保留整行**，你应该像上一章所解释的那样 **更新** 该行，将该列的特定值设置为 `NULL`（在 Python 中为 `None`）。

现在让我们用 **SQLModel** 来删除。

为了获得相同的结果，请在运行示例之前删除 `database.db` 文件。

## 从数据库读取

我们将首先选择之前章节中更新过的英雄 `"Spider-Youngster"`，这是我们接下来要删除的对象：

//// tab | Python 3.10+

```Python hl_lines="5"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001_py310.py[ln:70-75]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001.py[ln:72-77]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/delete/tutorial001.py!}
```

////

///

由于这是一个新函数 `delete_heroes()`，我们还需要将其添加到 `main()` 函数中，以便在从命令行执行程序时调用：

//// tab | Python 3.10+

```Python hl_lines="7"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001_py310.py[ln:90-98]!}
```

////

//// tab | Python 3.7+

```Python hl_lines="7"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001.py[ln:92-100]!}
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/delete/tutorial001.py!}
```

////

///

这将打印出当前存在的英雄 **Spider-Youngster**：

<div class="termy">

```console
$ python app.py

// 一些样板和之前的输出省略 😉

// 执行带 WHERE 的 SELECT
INFO Engine BEGIN (implicit)
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.name = ?
INFO Engine [no key 0.00011s] ('Spider-Youngster',)

// 打印从数据库获取的英雄
Hero:  name='Spider-Youngster' secret_name='Pedro Parqueador' age=16 id=2
```

</div>

## 从会话中删除英雄

现在，和我们使用 `session.add()` 来添加或更新新英雄的方式类似，我们可以使用 `session.delete()` 来从会话中删除英雄：

//// tab | Python 3.10+

```Python hl_lines="10"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001_py310.py[ln:70-77]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="10"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001.py[ln:72-79]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/delete/tutorial001.py!}
```

////

///

## 提交会话

要保存会话中的当前更改， **提交** 它。

这将保存会话中存储的所有更改，比如删除的英雄：

//// tab | Python 3.10+

```Python hl_lines="11"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001_py310.py[ln:70-78]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001.py[ln:72-80]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/delete/tutorial001.py!}
```

////

///

和之前看到的一样，`.commit()` 还会保存会话中所有其他的更改，包括更新或创建的英雄。

在删除英雄之后的提交将生成以下输出：

<div class="termy">

```console
$ python app.py

// 一些样板输出省略 😉

// 之前的输出省略 🙈

// 删除英雄的 SQL
INFO Engine DELETE FROM hero WHERE hero.id = ?
INFO Engine [generated in 0.00020s] (2,)
INFO Engine COMMIT
```

</div>

## 打印已删除的对象

现在，英雄已从数据库中删除。

如果我们尝试使用 `session.refresh()` 来刷新它，将会引发异常，因为数据库中没有这个英雄的数据。

不过，尽管如此，这个对象仍然存在并保留其数据，但现在它不再与会话连接，也不再存在于数据库中。

由于该对象不再与会话连接，它没有被标记为“过期”，会话也不再关心这个对象。

因此，该对象仍然包含其属性和数据，我们可以打印它：

//// tab | Python 3.10+

```Python hl_lines="13"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001_py310.py[ln:70-80]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="13"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001.py[ln:72-82]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/delete/tutorial001.py!}
```

////

///

这将输出：

<div class="termy">

```console
$ python app.py

// 一些样板输出省略 😉

// 之前的输出省略 🙈

// 打印已删除的英雄
Deleted hero: name='Spider-Youngster' secret_name='Pedro Parqueador' age=16 id=2
```

</div>

## 查询数据库中的相同行

为了确认它是否已被删除，现在我们再次查询数据库，使用相同的 `"Spider-Youngster"` 名字：

//// tab | Python 3.10+

```Python hl_lines="15-17"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001_py310.py[ln:70-84]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="15-17"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001.py[ln:72-86]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/delete/tutorial001.py!}
```

////

///

这里我们使用 `results.first()` 获取找到的第一个对象（如果找到了多个），或者如果没有找到任何内容，则返回 `None`。

如果我们使用 `results.one()`，则会引发异常，因为它期望恰好返回一个结果。

由于我们刚刚删除了该英雄，因此此查询应该找不到任何内容，应该返回 `None`。

这将执行一些 SQL 查询，并输出：

<div class="termy">

```console
$ python app.py

// 一些样板输出省略 😉

// 之前的输出省略 🙈

// 自动启动一个新事务
INFO Engine BEGIN (implicit)

// SQL 查询英雄
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age
FROM hero
WHERE hero.name = ?
INFO Engine [no key 0.00013s] ('Spider-Youngster',)
```

</div>

## 确认删除

现在让我们确认一下，确实没有找到数据库中名为 `"Spider-Youngster"` 的英雄。

我们可以通过检查 `results` 中的第一个项目是否为 `None` 来做到这一点：

//// tab | Python 3.10+

```Python hl_lines="19-20"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001_py310.py[ln:70-87]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="19-20"
# 上面的代码省略 👆

{!./docs_src/tutorial/delete/tutorial001.py[ln:72-89]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/delete/tutorial001.py!}
```

////

///

这将输出：

<div class="termy">

```console
$ python app.py

// 一些样板输出省略 😉

// 之前的输出省略 🙈

// 确实，英雄已被删除 🔥
没有名为 Spider-Youngster 的英雄

// 完成后清理 with 块
INFO Engine ROLLBACK
```

</div>

## 回顾代码

现在让我们回顾一下所有的代码：

//// tab | Python 3.10+

```{ .python .annotate hl_lines="70-88" }
{!./docs_src/tutorial/delete/tutorial002_py310.py!}
```

{!./docs_src/tutorial/delete/annotations/en/tutorial002.md!}

////

//// tab | Python 3.7+

```{ .python .annotate hl_lines="72-90" }
{!./docs_src/tutorial/delete/tutorial002.py!}
```

{!./docs_src/tutorial/delete/annotations/en/tutorial002.md!}

////

/// tip

查看编号气泡，了解每行代码做了什么。

///

## 小结

要使用 **SQLModel** 删除行，只需通过 **session** 调用 `.delete()` 删除它们，然后像往常一样，使用 `.commit()` 提交会话，将更改保存到数据库中。🔥
