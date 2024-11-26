# 多对多 - 介绍

我们已经了解了如何处理数据中的 <abbr title="也叫做多对一">一对多</abbr> 关系。

但是如何处理 **多对多** 关系呢？

让我们来探索一下。🚀

## 从一对多开始

我们先从熟悉且简单的 **一对多** 关系开始。

我们有一个包含团队的表和一个包含英雄的表，对于每个 **单一** 团队，我们可以有 **多个** 英雄。

由于每个团队可以有多个英雄，我们不能在 `team` 表中为每个英雄都创建一个专门的列来存放他们的 ID。

但是，由于每个英雄只能属于 **一个** 团队，因此在英雄表中，我们有 **一个单一的列** 来指向特定的团队（即指向 `team` 表中的特定行）。

`team` 表看起来像这样：

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

/// tip

注意，这里没有任何指向其他表的外键。

///

`hero` 表看起来像这样：

<table>
<tr>
<th>id</th><th>name</th><th>secret_name</th><th>age</th><th>team_id</th>
</tr>
<tr>
<td>1</td><td>Deadpond</td><td>Dive Wilson</td><td>null</td><td>2</td>
</tr>
<tr>
<td>2</td><td>Spider-Boy</td><td>Pedro Parqueador</td><td>null</td><td>1</td>
</tr>
<tr>
<td>3</td><td>Rusty-Man</td><td>Tommy Sharp</td><td>48</td><td>1</td>
</tr>
</table>

我们在 `hero` 表中有一个 `team_id` 列，它指向 `team` 表中某个特定团队的 ID。

这就是我们如何将每个 `hero` 与一个 `team` 连接起来：

<img alt="table relationships" src="../../../img/databases/relationships.svg">

注意，每个英雄只能有 **一个** 连接。但是每个团队可以接收 **多个** 连接。特别地，团队 **Preventers** 有两个英雄。

## 引入多对多

但假设 **Deadpond** 是一个伟大的角色，他被招募到新的 **Preventers** 团队，但他仍然是 **Z-Force** 团队的一员。

所以现在，我们需要能够让一个英雄连接到 **多个** 团队。并且，每个团队仍然可以接收 **多个** 英雄。因此，我们需要一个 **多对多** 关系。

一个不起作用的简单做法是往 `hero` 表中添加更多的列。假设我们添加了两个额外的列。那么现在我们可以将一个 `hero` 连接到最多 3 个团队，但不能更多。因此，这样做并没有真正解决支持 **多个** 团队的问题，只是支持了一个非常有限的固定数量的团队。

我们可以做得更好！ 🤓

## 链接表

我们可以创建另一个表来表示 `hero` 表和 `team` 表之间的链接。

这个表只包含两列：`hero_id` 和 `team_id`。

这两列都是 **外键**，分别指向 `hero` 表和 `team` 表中某个特定行的 ID。

因为这个表将表示 **英雄-团队链接**，我们可以将其命名为 `heroteamlink`。

它看起来是这样的：

<img alt="多对多表关系" src="../../../img/tutorial/many-to-many/many-to-many.svg">

注意，现在 `hero` 表中 **不再有 `team_id`** 列，它被这个链接表所替代。

而 `team` 表，像以前一样，仍然没有任何外键。

具体来说，新的链接表 `heroteamlink` 会是：

<table>
<tr>
<th>hero_id</th><th>team_id</th>
</tr>
<tr>
<td>1</td><td>1</td>
</tr>
<tr>
<td>1</td><td>2</td>
</tr>
<tr>
<td>2</td><td>1</td>
</tr>
<tr>
<td>3</td><td>1</td>
</tr>
</table>

/// info

此 **链接表** 的其他常用名称包括：

* 关联表（association table）
* 二级表（secondary table）
* 连接表（junction table）
* 中间表（intermediate table）
* 联接表（join table）
* 通过表（through table）
* 关系表（relationship table）
* 连接表（connection table）

我使用“链接表”这个术语，因为它简短、不与其他已用的术语（例如“关系”）冲突，且容易记住怎么写，等等。

///

## 链接主键

好，我们有一个只有 **两列** 的链接表。但记住，SQL 数据库要求每一行都必须有一个 **主键** 来 **唯一标识** 该表中的行，对吧？

那么，在这个表中， **主键** 是什么呢？

我们该如何标识每一行的唯一性？

是否需要再添加一列来作为这个链接表的 **主键** ？不！我们不需要这样做。👌

**这两列作为主键** ，共同标识这个表中的每一行（每一行只有这两列）。✨

主键是一种 **唯一标识** 单个表中特定行的方式。但它不一定是单独的一列。

主键可以是表中一组列，组合起来在表中是唯一的。

再看一下上面的表，看到 **每一行都有唯一的 `hero_id` 和 `team_id` 组合** 吗？

我们不能有重复的主键，这意味着我们不能有重复的 `hero` 和 `team` 之间的链接，这正是我们想要的！

例如，数据库现在会防止出现像下面这样有重复行的错误：

<table>
<tr>
<th>hero_id</th><th>team_id</th>
</tr>
<tr>
<td>1</td><td>1</td>
</tr>
<tr>
<td>1</td><td>2</td>
</tr>
<tr>
<td>2</td><td>1</td>
</tr>
<tr>
<td>3</td><td>1</td>
</tr>
<tr>
<td>3 🚨</td><td>1 🚨</td>
</tr>
</table>

让一个英雄 **重复加入同一个团队** 是没有意义的，对吧？

现在，只需使用这两列作为这个表的主键，SQL 就会自动处理 **防止我们重复插入** `hero` 和 `team` 之间的链接。✅

## 总结

一个总结性的介绍！这有点奇怪……不过没关系。🤷

现在你已经了解了 **多对多** 关系的理论，以及如何通过 SQL 表来解决它们。🤓

接下来，让我们看看如何编写 SQL 和代码来实现它们。🚀
