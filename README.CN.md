<p align="center">
  <a href="https://sqlmodel.tiangolo.com"><img src="https://sqlmodel.tiangolo.com/img/logo-margin/logo-margin-vector.svg#only-light" alt="SQLModel"></a>

</p>
<p align="center">
    <em>SQLModel, SQL databases in Python, designed for simplicity, compatibility, and robustness.</em>
</p>
<p align="center">
<a href="https://github.com/fastapi/sqlmodel/actions?query=workflow%3ATest" target="_blank">
    <img src="https://github.com/fastapi/sqlmodel/workflows/Test/badge.svg" alt="Test">
</a>
<a href="https://github.com/fastapi/sqlmodel/actions?query=workflow%3APublish" target="_blank">
    <img src="https://github.com/fastapi/sqlmodel/workflows/Publish/badge.svg" alt="Publish">
</a>
<a href="https://coverage-badge.samuelcolvin.workers.dev/redirect/fastapi/sqlmodel" target="_blank">
    <img src="https://coverage-badge.samuelcolvin.workers.dev/fastapi/sqlmodel.svg" alt="Coverage">
<a href="https://pypi.org/project/sqlmodel" target="_blank">
    <img src="https://img.shields.io/pypi/v/sqlmodel?color=%2334D058&label=pypi%20package" alt="Package version">
</a>
</p>

---

**文档**：<a href="https://sqlmodel.tiangolo.com" target="_blank">https://sqlmodel.tiangolo.com</a>

**源码**：<a href="https://github.com/fastapi/sqlmodel" target="_blank">https://github.com/fastapi/sqlmodel</a>

---

SQLModel 是一个用于通过 Python 代码与 <abbr title='也称为"关系型数据库"'>SQL 数据库</abbr> 交互的库，使用 Python 对象操作。它被设计得直观、易用、高度兼容且强大。

**SQLModel** 基于 Python 的类型注解，借助 <a href="https://pydantic-docs.helpmanual.io/" class="external-link" target="_blank">Pydantic</a> 和 <a href="https://sqlalchemy.org/" class="external-link" target="_blank">SQLAlchemy</a> 提供强大的功能。

主要特性包括：

* **编写直观**：拥有出色的编辑器支持。<abbr title="也称为自动完成、代码补全、智能提示">代码补全</abbr> 随处可见。减少调试时间。易于使用和学习，减少查阅文档的时间。
* **易于使用**：具有合理的默认配置，底层完成了大量工作，简化了你的代码编写。
* **兼容性强**：设计时充分考虑与 **FastAPI**、Pydantic 和 SQLAlchemy 的兼容性。
* **可扩展**：底层集成了 SQLAlchemy 和 Pydantic 的全部功能。
* **简洁**：最大程度减少代码重复。一处类型注解即可完成多种工作，无需在 SQLAlchemy 和 Pydantic 中重复定义模型。

## 赞助商

<!-- sponsors -->

<a href="https://www.govcert.lu" target="_blank" title="本项目由 GOVCERT.LU 提供支持"><img src="https://sqlmodel.tiangolo.com/img/sponsors/govcert.png"></a>

<!-- /sponsors -->

## 在 FastAPI 中使用 SQL 数据库

<a href="https://fastapi.tiangolo.com" target="_blank"><img src="https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png" style="width: 20%;"></a>

**SQLModel** 旨在简化 <a href="https://fastapi.tiangolo.com" class="external-link" target="_blank">FastAPI</a> 应用中与 SQL 数据库的交互，由同一 <a href="https://tiangolo.com/" class="external-link" target="_blank">作者</a> 创建。😁

它结合了 SQLAlchemy 和 Pydantic 的功能，尽可能简化你的代码，**最大限度减少代码重复**，同时提供 **最佳开发体验**。

实际上，**SQLModel** 是 **Pydantic** 和 **SQLAlchemy** 之上的一层轻量抽象，精心设计以保证与二者的兼容性。

## 要求

需要一个较新的且当前仍受支持的 <a href="https://www.python.org/downloads/" class="external-link" target="_blank">Python 版本</a>。

由于 **SQLModel** 基于 **Pydantic** 和 **SQLAlchemy**，因此需要它们作为依赖。安装 SQLModel 时会自动安装这些依赖。

## 安装

请确保创建并激活 <a href="https://sqlmodel.tiangolo.com/virtual-environments/" class="external-link" target="_blank">虚拟环境</a>，然后安装 SQLModel，例如：

<div class="termy">

```console
$ pip install sqlmodel
---> 100%
Successfully installed sqlmodel
```

</div>

## 示例

关于数据库、SQL 和其他相关内容的入门，请参阅 <a href="https://sqlmodel.tiangolo.com/databases/" target="_blank">SQLModel 文档</a>。

以下是一个简单示例。✨

### 一个 SQL 表

假设你有一个名为 `hero` 的 SQL 表，其中包含以下字段：

* `id`
* `name`
* `secret_name`
* `age`

并希望其数据如下所示：

| id | name         | secret_name    | age  |
|----|--------------|----------------|------|
| 1  | Deadpond     | Dive Wilson    | null |
| 2  | Spider-Boy   | Pedro Parqueador | null |
| 3  | Rusty-Man    | Tommy Sharp    | 48   | 

### 创建一个 SQLModel 模型

然后，您可以像下面这样创建一个 **SQLModel** 模型：

```Python
from typing import Optional

from sqlmodel import Field, SQLModel


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None
```

这个 `Hero` 类是一个 **SQLModel** 模型，相当于 Python 代码中的一个 SQL 表。

其中每个类属性对应于一个 **表列**。

### 创建行

接下来，您可以将表中的每一行作为模型的 **实例** 来创建：

```Python
hero_1 = Hero(name="Deadpond", secret_name="Dive Wilson")
hero_2 = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")
hero_3 = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)
```

通过这种方式，您可以使用常规的 Python 代码，通过 **类** 和 **实例** 来表示 **表** 和 **行**，从而与 **SQL 数据库** 交互。

### 编辑器支持

一切都设计为为您提供最佳的开发体验以及最佳的编辑器支持。

包括 **代码补全**：

<img class="shadow" src="https://sqlmodel.tiangolo.com/img/index/autocompletion01.png">

以及 **内联错误提示**：

<img class="shadow" src="https://sqlmodel.tiangolo.com/img/index/inline-errors01.png">

### 写入数据库

您可以通过快速学习 **教程** 了解更多关于 **SQLModel** 的内容，但如果您现在想了解如何将以上内容整合在一起并保存到数据库，可以这样做：

```Python hl_lines="18  21  23-27"
from typing import Optional

from sqlmodel import Field, Session, SQLModel, create_engine


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None


hero_1 = Hero(name="Deadpond", secret_name="Dive Wilson")
hero_2 = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")
hero_3 = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)


engine = create_engine("sqlite:///database.db")


SQLModel.metadata.create_all(engine)

with Session(engine) as session:
    session.add(hero_1)
    session.add(hero_2)
    session.add(hero_3)
    session.commit()
```

这会创建一个 **SQLite** 数据库并保存包含这 3 个英雄的数据。

### 从数据库中选择

然后，您可以编写查询来从同一个数据库中选择数据，例如：

```Python hl_lines="15-18"
from typing import Optional

from sqlmodel import Field, Session, SQLModel, create_engine, select


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None


engine = create_engine("sqlite:///database.db")

with Session(engine) as session:
    statement = select(Hero).where(Hero.name == "Spider-Boy")
    hero = session.exec(statement).first()
    print(hero)
```

### 编辑器支持无处不在

**SQLModel** 被精心设计以提供最佳的开发体验和编辑器支持，**甚至在从数据库选择数据之后**：

<img class="shadow" src="https://sqlmodel.tiangolo.com/img/index/autocompletion02.png">

## SQLAlchemy 和 Pydantic

这个 `Hero` 类是一个 **SQLModel** 模型。

但与此同时，✨ 它也是一个 **SQLAlchemy** 模型 ✨。因此，您可以将其与其他 SQLAlchemy 模型结合使用，或者轻松地将基于 SQLAlchemy 的应用程序迁移到 **SQLModel**。

与此同时，✨ 它还是一个 **Pydantic** 模型 ✨。您可以通过继承它来定义所有 **数据模型**，从而避免代码重复。这使得它非常容易与 **FastAPI** 一起使用。

## 许可证

此项目依据 [MIT 许可证](https://github.com/fastapi/sqlmodel/blob/main/LICENSE) 发布。
