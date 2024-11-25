# 使用 SQLModel 创建表 - 使用引擎

现在让我们开始编写代码吧。👩‍💻

确保你已经进入你的项目目录并激活了虚拟环境，正如[上一章节所解释的](index.md){.internal-link target=_blank}。

我们将：

* 使用 **SQLModel** 定义一个表
* 使用 **SQLModel** 创建相同的 SQLite 数据库和表
* 使用 **DB Browser for SQLite** 来确认操作

这里是我们想要的表结构的提醒：

<table>
<tr>
<th>id</th><th>name</th><th>secret_name</th><th>age</th>
</tr>
<tr>
<td>1</td><td>Deadpond</td><td>Dive Wilson</td><td>null</td>
</tr>
<tr>
<td>2</td><td>Spider-Boy</td><td>Pedro Parqueador</td><td>null</td>
</tr>
<tr>
<td>3</td><td>Rusty-Man</td><td>Tommy Sharp</td><td>48</td>
</tr>
</table>

## 创建表模型类

我们需要做的第一件事是创建一个类来表示表中的数据。

这样的类通常被称为 **模型**。

/// tip

这就是为什么这个包叫做 `SQLModel`。因为它主要用于创建 **SQL 模型**。

///

为此，我们将导入 `SQLModel`（以及我们还将使用的其他内容），并创建一个继承自 `SQLModel` 的类 `Hero`，它表示我们的英雄的 **表模型**：

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py ln[1:8] hl[1,4] *}

这个 `Hero` 类 **表示表**，我们稍后创建的每个实例将 **表示表中的一行**。

我们使用配置 `table=True` 来告诉 **SQLModel** 这是一个 **表模型**，它代表一个表。

/// info

也可以没有 `table=True` 的模型，那些只是 **数据模型**，没有数据库中的表，它们不会是 **表模型**。

这些 **数据模型** 在以后会非常有用，但现在，我们只需继续添加 `table=True` 配置。

///

## 定义字段、列

下一步是通过使用标准的 Python 类型注释来定义类的字段或列。

这些变量的名称将成为表中列的名称。

而每个字段的类型也将对应表列的类型：

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py ln[1:8] hl[1,5:8] *}

现在我们更详细地看一下这些字段/列的声明。

### 可选字段，允许为空的列

从 `age` 开始注意，它的类型是 `int | None`（或 `Optional[int]`）。

我们从 `typing` 标准模块导入了 `Optional`。

这是在 Python 中声明某个东西 "可以是 `int` 或 `None`" 的标准方式。

我们还将 `age` 的默认值设置为 `None`。

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py ln[1:8] hl[8] *}

/// tip

我们也将 `id` 定义为 `Optional`。但是我们将在下面讨论 `id`。

///

通过这种方式，我们告诉 **SQLModel** 在验证数据时，`age` 不是必需的，并且它的默认值是 `None`。

我们还告诉它，在 SQL 数据库中，`age` 的默认值是 `NULL`（与 Python 中的 `None` 等价）。

因此，这个列是 "可为空的"（可以设置为 `NULL`）。

/// info

在 **Pydantic** 中，`age` 是一个 **可选字段**。

在 **SQLAlchemy** 中，`age` 是一个 **允许为空的列**。

///

### 主键 `id`

现在让我们回顾一下 `id` 字段。它是表的 <abbr title="每一行在特定表中的唯一标识符。">**主键**</abbr>。

因此，我们需要将 `id` 标记为 **主键**。

为此，我们使用来自 `sqlmodel` 的特殊 `Field` 函数，并设置参数 `primary_key=True`：

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py ln[1:8] hl[1,5] *}

通过这种方式，我们告诉 **SQLModel** 这个 `id` 字段/列是表的主键。

但是在 SQL 数据库中，它是 **始终必须的**，并且不能为 `NULL`。为什么我们要用 `Optional` 来声明它呢？

`id` 在数据库中是必须的，但它将由数据库生成，而不是由我们的代码生成。

因此，每当我们创建这个类的实例（在后续章节中），我们 *不会设置 `id`*。并且 `id` 的值将是 `None` **直到我们将其保存到数据库中**，然后它才会最终有一个值。

```Python
my_hero = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")

do_something(my_hero.id)  # 哎呀！ my_hero.id 是 None！ 😱🚨

# 想象一下这将它保存到数据库中
somehow_save_in_db(my_hero)

do_something(my_hero.id)  # 现在 my_hero.id 有一个由数据库生成的值 🎉
```

因此，因为在 *我们的代码中*（而不是数据库中）`id` 的值 *可能是* `None`，所以我们使用 `Optional`。这样 **编辑器将能够帮助我们**，例如，如果我们尝试访问一个尚未保存到数据库中的对象的 `id`，它仍然会是 `None`。

<img class="shadow" src="../../img/create-db-and-table/inline-errors01.png">

现在，由于我们用 `Field()` 函数取代了默认值，我们在 `Field()` 中通过参数 `default=None` 设置了 `id` 的 **实际默认值**：

```Python
Field(default=None)
```

如果我们没有设置 `default` 值，每当我们稍后使用这个模型进行数据验证时（由 Pydantic 提供支持），它将 *接受* `None` 和 `int` 的值，但仍然 **要求** 传递那个 `None` 值。这对于后续使用该模型的人（可能是我们自己）来说会非常困惑，因此 **最好在这里设置默认值** 。

## 创建引擎

现在我们需要创建 SQLAlchemy **引擎**。

它是一个处理与数据库通信的对象。

如果你使用的是服务器数据库（例如 PostgreSQL 或 MySQL），**引擎**将保存与该数据库的 **网络连接**。

创建 **引擎** 非常简单，只需调用 `create_engine()` 并提供数据库的 URL：

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py ln[1:16] hl[1,14] *}

通常，你应该为整个应用程序创建一个 **引擎** 对象，并在各个地方重用它。

/// tip

还有一个相关的概念叫做 **会话**，它通常不应该是每个应用程序的单个对象。

但我们稍后会讨论这个问题。

///

### 引擎数据库 URL

每个支持的数据库都有其自己的 URL 类型。例如，对于 **SQLite**，它是 `sqlite:///` 后跟文件路径。例如：

* `sqlite:///database.db`
* `sqlite:///databases/local/application.db`
* `sqlite:///db.sqlite`

SQLite 支持一个特殊的数据库，它完全存在于 *内存中*。因此，它非常快速，但请小心，程序终止后数据库会被删除。你可以通过只使用两个斜杠字符（`//`）而不指定文件名来指定这个内存数据库：

* `sqlite://`

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py ln[1:16] hl[11:12,14] *}

你可以在 <a href="https://docs.sqlalchemy.org/en/14/core/engines.html" class="external-link" target="_blank">SQLAlchemy 文档</a> 中阅读更多关于 **SQLAlchemy** 支持的所有数据库的信息（从而也支持 **SQLModel**）。

### 引擎回显

在这个示例中，我们还使用了 `echo=True` 参数。

它会让引擎打印出它执行的所有 SQL 语句，这有助于你理解发生了什么。

它特别有助于 **学习** 和 **调试**：

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py ln[1:16] hl[14] *}

但在生产环境中，你可能希望去掉 `echo=True`：

```Python
engine = create_engine(sqlite_url)
```

### 引擎技术细节

/// tip

如果你之前对 SQLAlchemy 不熟悉，现在只是学习 **SQLModel**，你可以跳过这一部分，继续往下看。

///

你可以在 <a href="https://docs.sqlalchemy.org/en/14/tutorial/engine.html" class="external-link" target="_blank">SQLAlchemy 文档</a> 中阅读更多关于引擎的内容。

**SQLModel** 定义了它自己的 `create_engine()` 函数。它与 SQLAlchemy 的 `create_engine()` 相同，不同之处在于它默认使用 `future=True`（这意味着它使用的是 SQLAlchemy 最新版本 1.4 以及未来版本 2.0 的风格）。

而且 SQLModel 的版本 `create_engine()` 是内部进行类型注解的，因此你的编辑器将能够通过自动完成和内联错误来帮助你。

## 创建数据库和表

现在一切都准备就绪，可以最终创建数据库和表：

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py hl[16] *}

/// tip

创建引擎并不会创建 `database.db` 文件。

但一旦我们运行 `SQLModel.metadata.create_all(engine)`，它将创建 `database.db` 文件 **并且** 在该数据库中创建 `hero` 表。

这两个操作会在一个步骤中完成。

///

让我们详细解释一下：

```Python
SQLModel.metadata.create_all(engine)
```

### SQLModel 元数据

`SQLModel` 类有一个 `metadata` 属性。它是 `MetaData` 类的一个实例。

每当你创建一个继承自 `SQLModel` **并且配置了 `table = True`** 的类时，它会在这个 `metadata` 属性中注册。

因此，在最后一行中，`SQLModel.metadata` 已经注册了 `Hero`。

### 调用 `create_all()`

`SQLModel.metadata` 中的这个 `MetaData` 对象有一个 `create_all()` 方法。

它接收一个 **引擎**，并使用该引擎来创建数据库和所有在该 `MetaData` 对象中注册的表。

### SQLModel 元数据的顺序很重要

这也意味着你必须在创建继承自 `SQLModel` 的新模型类的代码 **之后** 调用 `SQLModel.metadata.create_all()`。

例如，假设你这样做：

* 在一个 Python 文件 `models.py` 中创建模型。
* 在文件 `db.py` 中创建引擎对象。
* 在主应用程序 `app.py` 中调用 `SQLModel.metadata.create_all()`。

如果你仅仅导入 `SQLModel` 并尝试在 `app.py` 中调用 `SQLModel.metadata.create_all()`，它将不会创建你的表：

```Python
# 这样做是行不通的！🚨
from sqlmodel import SQLModel

from .db import engine

SQLModel.metadata.create_all(engine)
```

之所以行不通，是因为当你单独导入 `SQLModel` 时，Python 并没有执行所有创建继承自它的类（在我们的示例中是 `Hero` 类）的代码，因此 `SQLModel.metadata` 仍然为空。

但是，如果你在调用 `SQLModel.metadata.create_all()` 之前导入模型，它将会生效：

```Python
from sqlmodel import SQLModel

from . import models
from .db import engine

SQLModel.metadata.create_all(engine)
```

这样做是有效的，因为通过导入模型，Python 会执行所有创建继承自 `SQLModel` 的类的代码，并将它们注册到 `SQLModel.metadata` 中。

作为一种替代方法，你可以在 `db.py` 中导入 `SQLModel` 和你的模型：

```Python
# db.py
from sqlmodel import SQLModel, create_engine
from . import models


sqlite_file_name = "database.db"
sqlite_url = f"sqlite:///{sqlite_file_name}"

engine = create_engine(sqlite_url)
```

然后在 `app.py` 中从 `db.py` 导入 `SQLModel`，并在其中调用 `SQLModel.metadata.create_all()`：

```Python
# app.py
from .db import engine, SQLModel

SQLModel.metadata.create_all(engine)
```

从 `db.py` 导入 `SQLModel` 是有效的，因为 `SQLModel` 也在 `db.py` 中被导入。

这个技巧能够正确地工作，并创建数据库中的表，因为通过从 `db.py` 导入 `SQLModel`，Python 会执行所有在 `db.py` 文件中创建继承自 `SQLModel` 的类的代码，例如 `Hero` 类。

## 数据库迁移

对于这个简单的示例，以及 **教程 - 用户指南** 中的大多数内容，使用 `SQLModel.metadata.create_all()` 就足够了。

但对于生产系统，你可能希望使用一个数据库迁移系统。

例如，每当你添加或删除列、添加新表、更改数据类型等时，这将非常有用和重要。

不过，你将在后面的高级用户指南中学习数据库迁移。

## 运行程序

现在运行程序，看看一切是否正常工作。

如果你还没有，将代码放在 `app.py` 文件中。

{* ./docs_src/tutorial/create_db_and_table/tutorial001_py310.py *}

/// tip

记得在运行之前 [激活虚拟环境](./index.md#create-a-python-virtual-environment){.internal-link target=_blank}。

///

现在使用 Python 运行程序：

<div class="termy">

```console
// 我们设置了 echo=True，所以这将显示 SQL 代码
$ python app.py

// 首先，一些我们不太关心的 SQL 样板代码

INFO Engine BEGIN (implicit)
INFO Engine PRAGMA main.table_info("hero")
INFO Engine [raw sql] ()
INFO Engine PRAGMA temp.table_info("hero")
INFO Engine [raw sql] ()
INFO Engine

// 最后，创建表的精彩 SQL ✨

CREATE TABLE hero (
        id INTEGER,
        name VARCHAR NOT NULL,
        secret_name VARCHAR NOT NULL,
        age INTEGER,
        PRIMARY KEY (id)
)

// 更多 SQL 样板代码

INFO Engine [no key 0.00020s] ()
INFO Engine COMMIT
```

</div>

/// info

为了使输出更易于阅读，我对上面的输出做了一些简化。

但实际上，它显示的内容可能是：

```
2021-07-25 21:37:39,175 INFO sqlalchemy.engine.Engine BEGIN (implicit)
```

///

### `TEXT` 或 `VARCHAR`

在上一章节的示例中，我们使用 `TEXT` 为某些列创建了表格。

但在这个输出中，SQLAlchemy 使用的是 `VARCHAR`。让我们看看发生了什么。

记住吗？[每个 SQL 数据库在支持的内容上都有一些不同的变种](../databases.md#sql-the-language){.internal-link target=_blank}？

这就是其中的一个差异。每个数据库支持某些特定的 **数据类型**，比如 `INTEGER` 和 `TEXT`。

一些数据库有一些特定的类型，专门用于某些特定的用途。例如，PostgreSQL 和 MySQL 支持 `BOOLEAN` 类型，用于表示 `True` 和 `False` 的值。SQLite 也接受带有布尔值的 SQL，甚至在定义表格列时也可以使用布尔值，但它内部实际使用的是 `INTEGER` 类型，用 `1` 表示 `True`，用 `0` 表示 `False`。

同样地，存储字符串有几种可能的类型。SQLite 使用 `TEXT` 类型，但像 PostgreSQL 和 MySQL 这样的其他数据库默认使用 `VARCHAR` 类型，`VARCHAR` 是最常见的字符串数据类型之一。

**`VARCHAR`** 来自 **可变** 长度 **字符**。

SQLAlchemy 生成的 SQL 语句使用 `VARCHAR`，然后 SQLite 接收这些语句，并内部将它们转换为 `TEXT`。

除了这两种数据类型之间的差异外，一些数据库，如 MySQL，要求为 `VARCHAR` 类型设置最大长度。例如，`VARCHAR(255)` 设置了字符数的最大值为 255。

为了使得 **SQLModel** 能够更方便地在任何数据库上立即使用（即使是 MySQL），并且不需要任何额外配置，默认情况下，`str` 类型的字段在大多数数据库中会被解释为 `VARCHAR`，在 MySQL 中则是 `VARCHAR(255)`。这样，你就可以确保同一个类可以兼容大多数流行数据库，无需额外的努力。

/// tip

你将在后续的高级教程 - 用户指南中学习如何更改字符串列的最大长度。

///

### 验证数据库

现在，使用 **DB Browser for SQLite** 打开数据库，你会看到程序已经创建了 `hero` 表，正如之前一样。🎉

<img class="shadow" src="../../img/create-db-and-table-with-db-browser/image008.png">

## 重构数据创建

现在，让我们稍微重构一下代码，使其更容易 **重用**、**共享** 和 **测试**。

我们将把那些主要有 **副作用**（即改变数据——创建一个数据库文件和表）的代码移到一个函数中。

在这个例子中，唯一的代码就是 `SQLModel.metadata.create_all(engine)`。

让我们将它放到一个名为 `create_db_and_tables()` 的函数中：

{* ./docs_src/tutorial/create_db_and_table/tutorial002_py310.py ln[1:18] hl[17:18] *}

如果 `SQLModel.metadata.create_all(engine)` 不在函数中，我们尝试从其他文件导入这个模块的内容时，它会 **每次** 执行导入该模块的文件时，都会尝试创建数据库和表格。

我们不希望这样发生，只有在我们 **打算** 这么做时才会执行，因此我们把它放到一个函数中，这样我们就可以确保只有在调用该函数时才会创建表格，而不是在模块被导入时自动执行。

现在，我们就可以在其他文件中导入 `Hero` 类，而不会发生那些 **副作用**。

/// tip

😅 **剧透**：这个函数被命名为 `create_db_and_tables()`，因为未来我们还会有其他 **表格**，包含除了 `Hero` 类以外的其他类。🚀

///

### 将数据创建作为脚本执行

我们已经防止了从 `app.py` 文件导入时的副作用。

但我们仍然希望当我们从终端直接以独立脚本的方式运行时，能够 **创建数据库和表格**，就像之前那样。

/// tip

可以将 **脚本** 和 **程序** 看作是可互换的。

**脚本** 这个词通常意味着代码可以独立且容易地运行，或者在某些情况下，它指的是一个相对简单的程序。

///

为此，我们可以在 `if` 语句块中使用特殊变量 `__name__`：

{* ./docs_src/tutorial/create_db_and_table/tutorial002_py310.py hl[21:22] *}

### 关于 `__name__ == "__main__"`

`__name__ == "__main__"` 的主要目的是在文件被直接执行时执行某些代码：

<div class="termy">

```console
$ python app.py

// 这里会发生一些事情 ✨
```

</div>

...但当其他文件导入它时，代码不会被执行，例如：

```Python
from app import Hero
```

/// tip

使用 `if __name__ == "__main__":` 的 `if` 语句块有时被称为 "**主块**"。

在 [Python 文档](https://docs.python.org/3/library/__main__.html) 中，官方名称是 "**顶级脚本环境**"。

///

#### 更多细节

假设你的文件名为 `myapp.py`。

如果你运行它：

<div class="termy">

```console
$ python myapp.py

// 这将调用 create_db_and_tables()
```

</div>

...那么 Python 自动创建的文件内部变量 `__name__` 的值将是字符串 `"__main__"`。

因此，以下函数将会执行：

```Python hl_lines="2"
if __name__ == "__main__":
    create_db_and_tables()
```

---

如果你导入了该模块（文件），就不会发生这种情况。

例如，如果你有另一个文件 `importer.py`，其中包含：

```Python
from myapp import Hero

# 其他代码
```

...在这种情况下，`myapp.py` 内部的自动变量 `__name__` 的值不会是 `"__main__"`。

因此，以下代码：

```Python hl_lines="2"
if __name__ == "__main__":
    create_db_and_tables()
```

...**不会** 执行。

/// info

欲了解更多信息，请查看 [官方 Python 文档](https://docs.python.org/3/library/__main__.html)。

///

## 最终回顾

在做了这些更改之后，你可以再次运行它，输出将与之前相同。

但现在我们可以从其他文件中导入该模块中的内容。

现在，让我们最终检查一下代码：

//// tab | Python 3.10+

```{.python .annotate}
{!./docs_src/tutorial/create_db_and_table/tutorial003_py310.py!}
```

{!./docs_src/tutorial/create_db_and_table/annotations/en/tutorial003.md!}

////

//// tab | Python 3.7+

```{.python .annotate}
{!./docs_src/tutorial/create_db_and_table/tutorial003.py!}
```

{!./docs_src/tutorial/create_db_and_table/annotations/en/tutorial003.md!}

////

/// tip

通过点击代码中的每个数字气泡，回顾每行代码的作用。👆

///

## 总结

我们学习了如何使用 **SQLModel** 来定义数据库中表格的结构，并且我们使用 **SQLModel** 创建了一个数据库和表格。

我们还重构了代码，使其更容易重用、共享和测试。

在接下来的章节中，我们将看到 **SQLModel** 如何帮助我们通过代码与 SQL 数据库进行交互。🤓
