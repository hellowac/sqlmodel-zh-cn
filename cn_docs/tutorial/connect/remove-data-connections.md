# 移除数据连接

当前，我们有一个 `team` 表：

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
<td>3</td><td>Spider-Boy</td><td>Pedro Parqueador</td><td>null</td><td>1</td>
</tr>
</table>

现在我们来看如何**移除**表之间行的连接。

我们将继续使用前一章节的代码。

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

## 断开连接

我们不需要真正删除任何数据来断开连接。我们可以只将外键字段的值设为 `None`，在这个例子中是将 `team_id` 设为 `None`。

假设 **Spider-Boy** 对于 **Preventers** 团队缺乏友好的邻居感到厌烦，想要退出这个团队。

我们只需要将 `team_id` 设置为 `None`，这样它就不再与该团队有连接了：

//// tab | Python 3.10+

```Python hl_lines="8"
# Code above omitted 👆

{!./docs_src/tutorial/connect/delete/tutorial001_py310.py[ln:29-30]!}

        # Previous code here omitted 👈

{!./docs_src/tutorial/connect/delete/tutorial001_py310.py[ln:66-70]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="8"
# Code above omitted 👆

{!./docs_src/tutorial/connect/delete/tutorial001.py[ln:31-32]!}

        # Previous code here omitted 👈

{!./docs_src/tutorial/connect/delete/tutorial001.py[ln:68-72]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/connect/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/connect/delete/tutorial001.py!}
```

////

///

同样，我们只是**赋值**给字段属性 `team_id`，将其设置为 `None`，在数据库中这意味着 `NULL`。然后我们将英雄对象 `add()` 到会话中，再通过 `commit()` 提交。

接下来，我们使用 `refresh()` 来获取最新的数据，并打印出来。

在命令行中运行后将输出：

<div class="termy">

```console
$ python app.py

// 之前的输出省略 😉

// 更新英雄
INFO Engine UPDATE hero SET team_id=? WHERE hero.id = ?
INFO Engine [cached since 0.07753s ago] (None, 3)
// 提交会话
INFO Engine COMMIT
// 自动启动一个新事务
INFO Engine BEGIN (implicit)
// 刷新英雄数据
INFO Engine SELECT hero.id, hero.name, hero.secret_name, hero.age, hero.team_id
FROM hero
WHERE hero.id = ?
INFO Engine [cached since 0.1661s ago] (3,)

// 打印没有团队的英雄
No longer Preventer: id=3 secret_name='Pedro Parqueador' team_id=None name='Spider-Boy' age=None
```

</div>

就这样，我们通过将外键列的值设为 `None`，成功移除了不同表之间的连接。💥