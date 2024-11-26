# 更新数据连接

此时，我们有一个 `team` 表：

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

以及一个 `hero` 表：

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

其中一些英雄属于某个团队。

现在我们将看到如何 **更新** 这些行之间的连接。

我们将继续使用创建英雄的代码，并对其进行更新。

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

## 为英雄分配一个团队

假设 **Tommy Sharp** 利用他“富有的叔叔”的魅力招募 **Spider-Boy** 加入 **Preventers** 团队，现在我们需要更新 **Spider-Boy** 英雄对象，将其连接到 **Preventers** 团队。

这样做就像更新任何其他字段一样：

//// tab | Python 3.10+

```Python hl_lines="8"
# Code above omitted 👆

{!./docs_src/tutorial/connect/update/tutorial001_py310.py[ln:29-30]!}

        # Previous code here omitted 👈

{!./docs_src/tutorial/connect/update/tutorial001_py310.py[ln:60-64]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8"
# Code above omitted 👆

{!./docs_src/tutorial/connect/update/tutorial001.py[ln:31-32]!}

        # Previous code here omitted 👈

{!./docs_src/tutorial/connect/update/tutorial001.py[ln:62-66]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/connect/update/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/connect/update/tutorial001.py!}
```

////

///

我们可以简单地 **分配** 一个值给字段属性 `team_id`，然后将英雄对象 `add()` 到会话中，再通过 `commit()` 提交。

接下来，我们使用 `refresh()` 来获取最新的数据，并打印它。

在命令行中运行后将输出：

<div class="termy">

```console
$ python app.py

// 之前的输出省略 😉

// 更新英雄
INFO Engine UPDATE hero SET team_id=? WHERE hero.id = ?
INFO Engine [generated in 0.00014s] (1, 3)
// 提交会话保存更改
INFO Engine COMMIT
// 自动启动一个新事务
INFO Engine BEGIN (implicit)
// 刷新英雄数据
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age, hero.team_id
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.08837s ago] (3,)

// 打印更新后的英雄
Updated hero: id=3 secret_name='Pedro Parqueador' team_id=1 name='Spider-Boy' age=None
```

</div>

现在 **Spider-Boy** 的 `team_id=1`，这是 **Preventers** 团队的 ID。🎉

接下来，我们将看到如何在下一章节中删除连接。💥
