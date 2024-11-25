# 特性

## 为 **FastAPI** 而设计

**SQLModel** 由 [FastAPI 的作者](https://tiangolo.com/) 创建。

<a href="https://fastapi.tiangolo.com" target="_blank"><img src="https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png" style="width: 20%;"></a>

它遵循相同的设计理念，旨在成为在 FastAPI 应用中与 SQL 数据库交互的最直观方式。

尽管如此，SQLModel 完全 **独立** 于 FastAPI，可以用于任何其他类型的应用中，依然能够从其特性中获益。

## 纯现代 Python

SQLModel 完全基于标准的 <abbr title="Python 当前支持的版本">现代 **Python**</abbr> 类型注解。无需学习新的语法，仅需使用标准的现代 Python。

如果您需要快速回顾如何使用 Python 类型注解（即使您不使用 SQLModel 或 FastAPI），可以查看 FastAPI 的教程部分：[Python 类型简介](https://fastapi.tiangolo.com/python-types/)。

您还可以在[教程 - 用户指南：第一步](tutorial/index.md){.internal-link target=_blank}部分找到一个 20 秒的快速回顾。

## 编辑器支持

**SQLModel** 设计易于使用且直观，以确保最佳的开发体验，同时实现全面的自动补全功能。

例如，在 <a href="https://code.visualstudio.com/" class="external-link" target="_blank">Visual Studio Code</a> 中：

<img class="shadow" src="/img/index/autocompletion02.png">

或者在 <a href="https://www.jetbrains.com/pycharm/" class="external-link" target="_blank">PyCharm</a> 中：

<img class="shadow" src="/img/features/autocompletion01.png">

您可以在编写**最少**代码时，获得全面的自动补全功能。

您无需猜测模型中不同属性的类型或它们是否可能为 `None`，因为 **SQLModel** 基于 **标准 Python 类型注解**，编辑器能够为您提供全方位的帮助。

**SQLModel** 采用 <a href="https://peps.python.org/pep-0681/" class="external-link" target="_blank">PEP 681</a> 来支持 Python 类型注解，从而确保最佳的开发者体验，因此即使在创建新的模型实例时，您也能获得内联错误提示和自动补全功能。

<img class="shadow" src="/img/index/autocompletion01.png">

## 简洁

**SQLModel** 为所有功能提供了**合理的默认值**，并在任何地方支持**可选配置**。

但默认情况下，所有功能都**“开箱即用”**。

您可以从数据的最简单（也是最直观）的类型注解开始。

之后，您可以使用 SQLAlchemy 和 Pydantic 的全部功能对其进行精细化配置。

## 基于 Pydantic

**SQLModel** 基于 Pydantic，并保持了相同的设计、语法和理念。

本质上，✨ 一个 **SQLModel** 模型也是一个 **Pydantic** 模型 ✨。

为此进行了大量的研究和努力。

这意味着您可以使用 **Pydantic** 的所有功能，包括自动的数据**验证**、**序列化**和**文档化**。您可以像使用 Pydantic 一样使用 SQLModel。

您甚至可以创建**不代表 SQL 表**的 SQLModel 模型。这种情况下，它们和 Pydantic 模型完全相同。

这特别有用，因为您现在可以创建一个从非 SQL 模型继承的 SQL 数据库模型。这样可以极大地**减少代码重复**，使代码更加一致，提升编辑器支持等。

这使得 SQLModel 成为在 **FastAPI** 应用中处理 SQL 数据库的完美组合。🚀

您将在教程的后续部分学习更多关于不同模型组合的内容。

## 基于 SQLAlchemy

**SQLModel** 同样基于 SQLAlchemy，所有功能的底层实现都依赖它。

本质上，✨ 一个 **SQLModel** 模型也是一个 **SQLAlchemy** 模型。✨

为了实现这一点，付出了**大量**的研究与努力。尤其是在让一个模型同时成为 **SQLAlchemy 模型和 Pydantic 模型**方面进行了大量尝试和实验。

这意味着您可以获得 SQLAlchemy 的所有功能、稳健性以及其作为 [Python 中最广泛使用的数据库库](https://www.jetbrains.com/lp/python-developers-survey-2020/) 所带来的可靠性。

**SQLModel** 提供了自己的工具来 <abbr title="支持类型补全、类型检查等功能">提升开发者体验</abbr>，但底层完全依赖 SQLAlchemy。

您甚至可以将 SQLModel 模型与 SQLAlchemy 模型**结合使用**。

SQLModel 的设计初衷是满足**最常见的使用场景**，并尽可能简化和方便这些场景下的开发，提供最佳的开发者体验。

但对于一些更复杂、更特殊的使用场景，您仍然可以直接在 SQLModel 中集成 SQLAlchemy，并在代码中使用它的所有功能。

## 经过测试

- **100%** <abbr title="代码中自动化测试覆盖的比例">测试覆盖率</abbr>（目前为 97%，将在未来几天/几周内达到 100%）。
- **100%** <abbr title="Python 类型注解，使编辑器和外部工具能够为您提供更好的支持">类型注解</abbr>的代码库。
