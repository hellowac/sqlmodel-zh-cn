# 十进制数字

在某些情况下，您可能需要能够存储具有精度保证的十进制数字。

这对于存储像 **货币**、**价格**、**账户** 等内容尤为重要，因为您希望确保不会发生四舍五入错误。

举个例子，如果您在 Python 中执行 `1.1 + 2.2`，您可能期望结果是 `3.3`，但实际上您会得到 `3.3000000000000003`：

```Python
>>> 1.1 + 2.2
3.3000000000000003
```

这是因为数字在“零和一”（二进制）中存储的方式。但 Python 有一个模块和一些类型，允许使用严格的十进制值。您可以在官方的 <a href="https://docs.python.org/3/library/decimal.html" class="external-link" target="_blank">Python Decimal 文档</a> 中了解更多内容。

由于数据库以与计算机相同的方式存储数据（即二进制），它们也会面临相同的问题。因此，数据库也有一个特殊的 **十进制** 类型。

在大多数情况下，这可能不会成为问题，例如视频中的观看次数或视频游戏中的生命条数。但正如您可以想象的那样，在处理 **货币** 和 **财务** 时，这一点尤为重要。

## 十进制类型

Pydantic 特别支持 <a href="https://docs.pydantic.dev/latest/api/standard_library_types/#decimaldecimal" class="external-link" target="_blank">`Decimal` 类型</a>。

当您使用 `Decimal` 时，可以在 `Field()` 函数中指定支持的位数和小数位数。Pydantic 会验证这些（例如，在使用 FastAPI 时），同样的信息也将用于数据库列。

/// info

对于数据库，**SQLModel** 将使用 <a href="https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.DECIMAL" class="external-link" target="_blank">SQLAlchemy 的 `DECIMAL` 类型</a>。

///

## SQLModel 中的十进制

假设数据库中每个英雄都有一笔钱。我们可以使用 `condecimal()` 函数将该字段设置为 `Decimal` 类型：

//// tab | Python 3.10+

```python hl_lines="11"
{!./docs_src/advanced/decimal/tutorial001_py310.py[ln:1-11]!}

# More code here later 👇
```

////

//// tab | Python 3.7+

```python hl_lines="12"
{!./docs_src/advanced/decimal/tutorial001.py[ln:1-12]!}

# More code here later 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/advanced/decimal/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/advanced/decimal/tutorial001.py!}
```

////

///

这里我们声明 `money` 最多可以有 `5` 位数字（通过 `max_digits`），**这包括整数部分**（小数点左边）**和小数部分**（小数点右边）。

我们还声明小数部分（小数点右边）的位数为 `3`，所以在 `money` 字段中，我们可以有 **3 位小数数字**。这意味着我们将有 **2 位用于整数部分** 和 **3 位用于小数部分**。

✅ 例如，以下都是 `money` 字段的有效数字：

* `12.345`
* `12.3`
* `12`
* `1.2`
* `0.123`
* `0`

🚫 但是以下数字对于 `money` 字段来说是无效的：

* `1.2345`
  * 这个数字有超过 3 位的小数。
* `123.234`
  * 这个数字的总位数（整数部分和小数部分）超过了 5 位。
* `123`
  * 即使这个数字没有小数部分，我们仍然为小数部分预留了 3 位，这意味着我们只能为 **整数部分** 使用 2 位，而这个数字有 3 位整数。所以，允许的整数位数是 `max_digits` - `decimal_places` = 2。

/// 提示

确保根据您自己应用程序的需求调整位数和小数位数。🤓

///

## 使用十进制创建模型

在创建新模型时，您实际上可以传递普通的 (`float`) 数字，Pydantic 会自动将它们转换为 `Decimal` 类型，**SQLModel** 会将它们作为 `Decimal` 类型存储在数据库中（使用 SQLAlchemy）。

//// tab | Python 3.10+

```Python hl_lines="4-6"
# Code above omitted 👆

{!./docs_src/advanced/decimal/tutorial001_py310.py[ln:24-34]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="4-6"
# Code above omitted 👆

{!./docs_src/advanced/decimal/tutorial001.py[ln:25-35]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/advanced/decimal/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/advanced/decimal/tutorial001.py!}
```

////

///

## 选择十进制数据

然后，当使用十进制类型时，您可以确认它们确实避免了浮动数字的四舍五入错误：

//// tab | Python 3.10+

```Python hl_lines="15-16"
# Code above omitted 👆

{!./docs_src/advanced/decimal/tutorial001_py310.py[ln:37-50]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="15-16"
# Code above omitted 👆

{!./docs_src/advanced/decimal/tutorial001.py[ln:38-51]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/advanced/decimal/tutorial001_py310.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/advanced/decimal/tutorial001.py!}
```

////

///

## 审查结果

现在，如果您运行此代码，它将输出 `3.300`，而不是意外的 `3.3000000000000003`：

<div class="termy">

```console
$ python app.py

// 一些示例代码和前面的输出省略 😉

// money 的类型是 Decimal('1.100')
Hero 1: id=1 secret_name='Dive Wilson' age=None name='Deadpond' money=Decimal('1.100')

// 更多输出省略 🤓

// money 的类型是 Decimal('1.100')
Hero 2: id=3 secret_name='Tommy Sharp' age=48 name='Rusty-Man' money=Decimal('2.200')

// 没有四舍五入错误，只有 3.3！🎉
Total money: 3.300
```

</div>

/// warning

尽管在 Python 端支持并使用了十进制类型，但并非所有数据库都支持它。特别是，SQLite 不支持十进制，因此它会将其转换为它支持的浮动 `NUMERIC` 类型。

但大多数其他 SQL 数据库都支持十进制。🎉

///
