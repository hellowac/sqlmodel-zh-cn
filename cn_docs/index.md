<style>
.md-content .md-typeset h1 { display: none; }
</style>

<p align="center">
  <a href="https://sqlmodel.tiangolo.com"><img src="https://sqlmodel.tiangolo.com/img/logo-margin/logo-margin-vector.svg#only-light" alt="SQLModel"></a>
<!-- only-mkdocs -->
  <a href="https://sqlmodel.tiangolo.com"><img src="img/logo-margin/logo-margin-white-vector.svg#only-dark" alt="SQLModel"></a>
<!-- /only-mkdocs -->
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

**源代码**：<a href="https://github.com/fastapi/sqlmodel" target="_blank">https://github.com/fastapi/sqlmodel</a>

---

SQLModel 是一个用于从 Python 代码中通过 Python 对象与 <abbr title="也称为关系型数据库">SQL 数据库</abbr> 交互的库。它旨在直观、易用、高度兼容且健壮。

**SQLModel** 基于 Python 类型注解，由 <a href="https://pydantic-docs.helpmanual.io/" class="external-link" target="_blank">Pydantic</a> 和 <a href="https://sqlalchemy.org/" class="external-link" target="_blank">SQLAlchemy</a> 提供支持。

其主要功能包括：

- **直观易写**：提供极佳的编辑器支持，<abbr title="也称为自动补全、代码补全、IntelliSense">代码补全</abbr>无处不在。减少调试时间，易于使用和学习，无需频繁查阅文档。
- **易于使用**：具备合理的默认配置，简化代码编写。
- **兼容性强**：与 **FastAPI**、Pydantic 和 SQLAlchemy 完全兼容。
- **可扩展性强**：底层拥有 SQLAlchemy 和 Pydantic 的强大功能。
- **简洁代码**：最小化代码重复。一个类型注解可以完成很多工作，无需在 SQLAlchemy 和 Pydantic 中重复定义模型。

## 赞助商

<!-- 赞助商 -->

{% if sponsors %}
{% for sponsor in sponsors.gold -%}
<a href="{{ sponsor.url }}" target="_blank" title="{{ sponsor.title }}"><img src="{{ sponsor.img }}" style="border-radius:15px"></a>
{% endfor -%}
{%- for sponsor in sponsors.silver -%}
<a href="{{ sponsor.url }}" target="_blank" title="{{ sponsor.title }}"><img src="{{ sponsor.img }}" style="border-radius:15px"></a>
{% endfor %}
{% endif %}

<!-- /赞助商 -->

## 在 FastAPI 中使用 SQL 数据库

<a href="https://fastapi.tiangolo.com" target="_blank"><img src="https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png" style="width: 20%;"></a>

**SQLModel** 旨在简化在 <a href="https://fastapi.tiangolo.com" class="external-link" target="_blank">FastAPI</a> 应用中与 SQL 数据库的交互，它由同一位 <a href="https://tiangolo.com/" class="external-link" target="_blank">作者</a> 创建。😁

它结合了 SQLAlchemy 和 Pydantic，尽可能简化代码编写，减少 **代码重复到最低限度**，同时提供 **最佳的开发者体验**。

实际上，**SQLModel** 是建立在 **Pydantic** 和 **SQLAlchemy** 之上的一层轻量封装，经过精心设计，确保与两者兼容。

## 环境要求

需要一个近期且仍受支持的 <a href="https://www.python.org/downloads/" class="external-link" target="_blank">Python 版本</a>。

由于 **SQLModel** 基于 **Pydantic** 和 **SQLAlchemy**，因此需要这两个依赖。在安装 SQLModel 时会自动安装它们。

## 安装

请确保创建并激活一个 <a href="https://sqlmodel.tiangolo.com/virtual-environments/" class="external-link" target="_blank">虚拟环境</a>，然后安装 SQLModel，例如：

<div class="termy">

```console
$ pip install sqlmodel
---> 100%
Successfully installed sqlmodel
```

</div>

## 示例

有关数据库、SQL 和其他相关内容的介绍，请参阅 <a href="https://sqlmodel.tiangolo.com/databases/" target="_blank">SQLModel 文档</a>。

以下是一个简单的示例。✨

### 一个 SQL 表

假设您有一个名为 `hero` 的 SQL 表，包含以下字段：

- `id`
- `name`
- `secret_name`
- `age`

并希望包含如下数据：

| id | name       | secret_name     | age  |
|----|------------|-----------------|------|
| 1  | Deadpond   | Dive Wilson     | null |
| 2  | Spider-Boy | Pedro Parqueador| null |
| 3  | Rusty-Man  | Tommy Sharp     | 48   |

### 创建一个 SQLModel 模型

您可以像这样创建一个 **SQLModel** 模型：

```Python
from typing import Optional

from sqlmodel import Field, SQLModel


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None
```

这个类 `Hero` 是一个 **SQLModel** 模型，等同于 Python 代码中的 SQL 表。

每个类属性对应一个 **表列**。

### 创建数据行

然后，您可以通过模型的 **实例** 来创建表的 **每一行**：

```Python
hero_1 = Hero(name="Deadpond", secret_name="Dive Wilson")
hero_2 = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")
hero_3 = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)
```

通过这种方式，您可以使用常规 Python 代码中的 **类** 和 **实例** 来表示 **表** 和 **行**，并与 **SQL 数据库** 交互。

### 编辑器支持

所有设计旨在为您提供最佳的开发者体验以及编辑器支持。

包括 **自动补全**：

<img class="shadow" src="https://sqlmodel.tiangolo.com/img/index/autocompletion01.png">

以及 **内联错误提示**：

<img class="shadow" src="https://sqlmodel.tiangolo.com/img/index/inline-errors01.png">

### 写入数据库

通过快速浏览 **教程**，您可以了解更多关于 **SQLModel** 的内容，但如果您想快速尝试如何将这些组合在一起并保存到数据库中，可以这样做：

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

此代码会保存一个包含 3 位英雄的 **SQLite** 数据库。

### 从数据库中查询

接下来，您可以通过编写查询从同一数据库中进行查询，例如：

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

### 全面支持编辑器功能

**SQLModel** 经过精心设计，可为您提供最佳的开发者体验和编辑器支持，**即使是在从数据库选择数据之后**：

<img class="shadow" src="https://sqlmodel.tiangolo.com/img/index/autocompletion02.png">

## SQLAlchemy 和 Pydantic

类 `Hero` 是一个 **SQLModel** 模型。

但同时，✨ 它也是一个 **SQLAlchemy** 模型 ✨。因此，您可以将其与其他 SQLAlchemy 模型结合使用，或者轻松地将基于 SQLAlchemy 的应用迁移到 **SQLModel**。

与此同时，✨ 它也是一个 **Pydantic** 模型 ✨。您可以使用继承来定义所有的 **数据模型**，从而避免代码重复。这使得它在 **FastAPI** 中非常易于使用。

## 许可证

此项目根据 [MIT 许可证](https://github.com/fastapi/sqlmodel/blob/main/LICENSE) 授权。
