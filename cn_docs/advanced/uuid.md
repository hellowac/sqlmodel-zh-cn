# UUID（通用唯一标识符）

我们已经讨论了一些数据类型，如 `str`、`int` 等。

另一个数据类型是 `UUID`（通用唯一标识符）。

你可能在 URL 中见过 **UUIDs**，它们看起来像这样：

```
4ff2dab7-bffe-414d-88a5-1826b9fea8df
```

UUIDs 可以作为 **主键** 的替代，用来代替自动递增的整数。

/// info

UUIDs 的官方支持在 SQLModel 版本 `0.0.20` 中被添加。

///

## 关于 UUIDs

UUIDs 是 128 位的数字，即 16 字节。

它们通常表示为由 32 个 **十六进制** 字符组成，这些字符由短横线分隔。

UUID 有多个版本，其中一些版本将当前时间包含在字节中，但 **UUID 版本 4** 主要是随机的，它们的生成方式使得它们几乎是 **唯一的**。

### 分布式 UUIDs

你可以在一台计算机上生成一个 UUID，在另一台计算机上生成另一个 UUID，它们几乎 **不可能** 完全相同。

这意味着，你无需等待数据库为你生成 ID，可以在代码中 **提前生成 UUID 并发送到数据库**，因为你可以非常确定它是唯一的。

/// note | 技术细节

由于可能的 UUID 数量非常大（2^128），因此生成两个相同的 UUID 版本 4（即随机生成的 UUID）的概率非常低。

如果你在数据库中存储了 103 万亿个版本 4 的 UUID，那么生成一个重复 UUID 的概率是十亿分之一。 🤓

///

出于同样的原因，如果你决定迁移数据库，将其与另一个数据库合并或混合记录等，你很可能 **可以直接使用原来的 UUIDs**。

/// warning

尽管发生碰撞的几率非常低，但它仍然存在。在大多数情况下，你可以假设不会发生碰撞，但做好准备总是好的。

///

### UUIDs 防止信息泄露

由于 UUID 版本 4 是 **随机的**，你可以将这些 ID 发送给应用程序用户或其他系统，**而不会暴露** 应用程序的任何信息。

使用 **自动递增的整数** 作为主键时，可能会无意中暴露系统中的信息。例如，某人可以创建一个新的英雄，并通过获取英雄 ID `20` **推断出系统中有 20 个英雄**（如果某些英雄已被删除，实际数量可能更少）。

### UUID 存储

由于 UUID 是 16 字节，它们在数据库中 **占用的空间比较大**，比自动递增的整数（通常为 4 字节）要多。

根据你使用的数据库，UUIDs 的 **性能和空间使用** 可能会有所不同。如果你关心这些问题，应该查阅特定数据库的文档。

SQLite 没有专门的 UUID 类型，因此它会将 UUID 存储为字符串。其他数据库，如 Postgres，有专门的 UUID 类型，这将比字符串更节省空间且性能更好。

## 使用 UUID 的模型

为了使用 UUID 作为主键，我们需要导入 `uuid`，它是 Python 标准库的一部分（不需要额外安装），并使用 `uuid.UUID` 作为 ID 字段的 **类型**。

我们还希望 Python 代码 **在创建新实例时生成新的 UUID**，所以我们使用 `default_factory`。

`default_factory` 参数接受一个函数（或者一般来说是一个 “可调用” 对象）。这个函数会在创建模型的新实例时 **被调用**，并返回的值将作为字段的默认值。

对于 `default_factory` 中的函数，我们传递 `uuid.uuid4`，这是一个生成 **新的 UUID 版本 4** 的函数。

/// tip

我们在代码中不会自己调用 `uuid.uuid4()`（不会加括号）。相反，我们只传递函数本身，`uuid.uuid4`，以便 SQLModel 每次创建新实例时都能调用它。

///

这意味着 UUID 会在 Python 代码中生成，**在将数据发送到数据库之前**。

{* ./docs_src/advanced/uuid/tutorial001_py310.py ln[1:10] hl[1,7] *}

Pydantic 支持 <a href="https://docs.pydantic.dev/latest/api/standard_library_types/#uuid" class="external-link" target="_blank">`UUID` 类型</a>。

对于数据库，**SQLModel** 内部使用 <a href="https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.Uuid" class="external-link" target="_blank">SQLAlchemy 的 `Uuid` 类型</a>。

### 使用 UUID 创建记录

创建 `Hero` 记录时，`id` 字段会 **自动填充** 新的 UUID，因为我们设置了 `default_factory=uuid.uuid4`。

由于 `uuid.uuid4` 会在创建模型实例时被调用，所以即使在将数据发送到数据库之前，我们也可以 **立即访问并使用这个 ID**。

而这个 **相同的 ID（UUID）** 会被保存到数据库中。

{* ./docs_src/advanced/uuid/tutorial001_py310.py ln[23:34] hl[25,27,29,34] *}

### 选择一个英雄

我们可以对 UUID 进行与其他字段相同的操作。

例如，我们可以 **通过 ID 选择一个英雄**：

{* ./docs_src/advanced/uuid/tutorial001_py310.py ln[37:54] hl[49] *}

/// tip

即使像 SQLite 这样的数据库将 UUID 存储为字符串，我们也可以使用 Python 的 UUID 对象进行选择和比较，它仍然有效。

SQLModel（实际上是 SQLAlchemy）会处理这些操作。✨

///

#### 使用 `session.get()` 选择

我们也可以使用 `session.get()` 按 ID 进行选择：

{* ./docs_src/advanced/uuid/tutorial002_py310.py ln[37:53] hl[49] *}

像处理其他字段一样，我们也可以更新、删除等。🚀

### 运行程序

如果运行程序，你将看到 **UUID** 在 Python 代码中生成，并且记录 **以相同的 UUID 保存到数据库** 中。

<div class="termy">

```console
$ python app.py

// 一些模板和前面的输出省略 😉

// 在 SQLite 中，UUID 会作为字符串存储
// 其他数据库如 Postgres 有专门的 UUID 类型
CREATE TABLE hero (
        id CHAR(32) NOT NULL,
        name VARCHAR NOT NULL,
        secret_name VARCHAR NOT NULL,
        age INTEGER,
        PRIMARY KEY (id)
)

// 在保存到数据库之前，我们已经有了 UUID
The hero before saving in the DB
name='Deadpond' secret_name='Dive Wilson' id=UUID('0e44c1a6-88d3-4a35-8b8a-307faa2def28') age=None
The hero ID was already set
0e44c1a6-88d3-4a35-8b8a-307faa2def28

// 插入记录的 SQL 语句使用了我们创建的 UUID
INSERT INTO hero (id, name, secret_name, age) VALUES (?, ?, ?, ?)
('0e44c1a688d34a358b8a307faa2def28', 'Deadpond', 'Dive Wilson', None)

// 记录确实使用我们创建的 UUID 保存了 😎
After saving in the DB
age=None id=UUID('0e44c1a6-88d3-4a35-8b8a-307faa2def28') name='Deadpond' secret_name='Dive Wilson'

// 现在我们创建一个新英雄（稍后选择）
Created hero:
age=None id=UUID('9d90d186-85db-4eaa-891a-def7b4ae2dab') name='Spider-Boy' secret_name='Pedro Parqueador'
Created hero ID:
9d90d186-85db-4eaa-891a-def7b4ae2dab

// 然后我们选择它
Selected hero:
age=None id=UUID('9d90d186-85db-4eaa-891a-def7b4ae2dab') name='Spider-Boy' secret_name='Pedro Parqueador'
Selected hero ID:
9d90d186-85db-4eaa-891a-def7b4ae2dab
```

</div>

## 了解更多

你可以了解更多关于 **UUIDs** 的信息：

* 官方的 <a href="https://docs.python.org/3/library/uuid.html" class="external-link" target="_blank">Python UUID 文档</a>。
* <a href="https://en.wikipedia.org/wiki/Universally_unique_identifier" class="external-link" target="_blank">Wikipedia 关于 UUID 的介绍</a>。