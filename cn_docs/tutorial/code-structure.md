# 代码结构与多个文件

让我们停下来思考一下如何组织代码，特别是在 **大型项目** 中，涉及多个文件的情况。

## 循环导入

`Hero` 类内部引用了 `Team` 类。

但是，`Team` 类也引用了 `Hero` 类。

因此，如果这两个类分别位于不同的文件中，并且你尝试直接在彼此的文件中导入这些类，就会导致 **循环导入** 。🔄

Python 无法处理这种情况，会抛出一个错误。🚨

但实际上，我们希望表达的是 **这种循环引用** ，因为在我们的代码中，我们可以做一些非常炫酷的事情，例如：

```Python
hero.team.heroes[0].team.heroes[1].team.heroes[2].name
```

这个循环引用正是我们通过这些 **关系属性** 在表达的意思，即：

* 一个英雄可以有一个团队
    * 这个团队可以有一组英雄
        * 这些英雄中的每个都可以有一个团队
            * ...以此类推。

接下来，我们将看到几种 **结构化代码** 的策略，考虑到这种情况。

## 单一模块模型

这是最简单的方式。✨

在这种解决方案中，我们仍然使用 **多个文件** ，分别用于 `models`、`database` 和 `app`。

我们还可以有任何其他必要的 **文件** 。

但在这种情况下，所有的模型都会放在 **一个文件** 中。

项目的文件结构可能是：

```
.
├── project
    ├── __init__.py
    ├── app.py
    ├── database.py
    └── models.py
```

我们有 3 个 **Python模块** （或文件）：

* `app`
* `database`
* `models`

我们还会有一个空的 `__init__.py` 文件，使该项目成为一个“ **Python 包** ”（一组 Python 模块）。这样，我们就可以在 `app.py` 文件/模块中使用 **相对导入** ，比如：

```Python
from .models import Hero, Team
from .database import engine
```

我们之所以能使用这些相对导入，是因为，例如，在文件 `app.py`（`app` 模块）中，Python 知道它是 **我们 Python 包的一部分** ，因为它与 `__init__.py` 文件位于同一目录。而同一目录下的所有 Python 文件也都属于同一个 Python 包。

### 模型文件

你可以将所有数据库模型放在一个 Python 模块（一个 Python 文件）中，例如 `models.py`：

```Python
{!./docs_src/tutorial/code_structure/tutorial001/models.py!}
```

这样，你就不需要为其他模型处理循环导入的问题。

然后，你可以在应用程序中的任何其他文件/模块中导入该文件/模块中的模型。

### 数据库文件

然后，你可以将创建**engine** 和创建所有表的函数（如果没有使用迁移的话）放在另一个文件 `database.py` 中：

```Python
{!./docs_src/tutorial/code_structure/tutorial001/database.py!}
```

这个文件也会被你的应用程序代码导入，以便使用共享的 **engine** ，并调用函数 `create_db_and_tables()`。

### 应用程序文件

最后，你可以将创建 **应用程序** 的代码放在另一个文件 `app.py` 中：

```Python hl_lines="3-4"
{!./docs_src/tutorial/code_structure/tutorial001/app.py!}
```

在这里，我们导入模型、engine 以及创建所有表的函数，然后可以在内部使用它们。

### 顺序很重要

记得在调用 `SQLModel.metadata.create_all()` 时， **顺序很重要** 吗？ [文档中的这一部分](create-db-and-table.md#sqlmodel-metadata-order-matters) 指出，你必须在调用 `SQLModel.metadata.create_all()` 之前，导入包含模型的模块。

我们在这里做的是，首先在 `app.py` 中导入模型，然后 **再** 创建数据库和表格，所以一切都正常，代码也能正确运行。👌

### 在命令行运行

因为现在这是一个包含 **Python 包** 的较大项目，而不是一个单一的 Python 文件，所以我们 **不能** 像以前那样只传递单个文件名来运行：

```console
$ python app.py
```

现在，我们需要告诉 Python，我们希望它执行一个作为包一部分的*模块*：

```console
$ python -m project.app
```

`-m` 选项告诉 Python 调用一个*模块*。接下来，我们传递 `project.app` 字符串，这是我们在 **导入** 时使用的相同格式：

```Python
import project.app
```

然后，Python 会在该包内执行该模块，并且由于 Python 是直接执行它的，`app.py` 中的 **主函数块** （main block）仍然会起作用：

```Python
if __name__ == '__main__':
    main()
```

所以，输出将是：

<div class="termy">

```console
$ python -m project.app

Created hero: id=1 secret_name='Dive Wilson' team_id=1 name='Deadpond' age=None
Hero's team: name='Z-Force' headquarters='Sister Margaret's Bar' id=1
```

</div>

## 解决循环导入问题

假设由于某种原因，你不喜欢将所有数据库模型放在一个文件中，而是希望将它们分开，分别放在 `hero_model.py` 和 `team_model.py` 文件中。

你当然可以这样做。😎 但有几件事需要注意。🤓

/// 警告

这有点更高级。

如果上面的解决方案已经适用于你，那可能就足够了，你可以继续进行下一章的内容。🤓

///

假设现在文件结构变为：

```
.
├── project
    ├── __init__.py
    ├── app.py
    ├── database.py
    ├── hero_model.py
    └── team_model.py
```

### 循环导入和类型注解

循环导入的问题在于，Python 无法在 **运行时** 解决它们。

但是，在使用 Python **类型注解** 时，通常需要声明一些变量的类型，这些变量的类可能是从其他文件导入的。

而这些包含类的文件 **也可能需要导入** 更多来自第一个文件的内容。

这最终就需要使用 **循环导入** ，而 Python 在 **运行时** 是不支持的。

### 类型注解与运行时

但这些我们想声明的 **类型注解** 并不需要在 **运行时** 使用。

事实上，记得我们使用了 `List["Hero"]`，其中 `"Hero"` 是一个字符串吗？

对 Python 来说，在运行时，这只是一个 **字符串**。

所以，如果我们能用 **字符串版本** 添加需要的类型注解，Python 就不会有问题。

但如果我们仅在类型注解中使用字符串，而不导入任何东西，编辑器就无法知道我们的意思，也无法提供 **自动补全** 和 **内联错误** 的帮助。

因此，如果有一种方法可以“导入”某些内容，只在编辑代码时作为“导入”，而在 **运行时** 不需要导入，那就可以解决这个问题……而这种方法确实存在！就是这样。🎉

### 仅在编辑时导入 `TYPE_CHECKING`

为了解决这个问题，Python 提供了一个特殊的技巧，利用 `typing` 模块中的一个特殊变量 `TYPE_CHECKING`。

该变量在代码编辑器和工具分析类型注解时值为 `True`。

但当 Python 执行时，值为 `False`。

因此，我们可以在 `if` 块中使用它，在其中导入其他内容。这样，这些内容只会在编辑器中“导入”，而在运行时不会导入。

### Hero 模型文件

使用 `TYPE_CHECKING` 的技巧，我们可以在 `hero_model.py` 中“导入” `Team`：

```Python hl_lines="1  5-6  16"
{!./docs_src/tutorial/code_structure/tutorial002/hero_model.py!}
```

请注意，现在我们 *必须* 将 `Team` 的注解写成字符串形式：“`"Team"`”，这样 Python 在运行时就不会报错。

### Team 模型文件

我们在 `team_model.py` 文件中使用相同的技巧：

```Python hl_lines="1  5-6  14"
{!./docs_src/tutorial/code_structure/tutorial002/team_model.py!}
```

现在我们可以在编辑器中得到支持，包括自动补全、内联错误提示，同时 **SQLModel** 仍然能够正常工作。🎉

### 应用程序文件

现在，为了完整性，`app.py` 文件将从两个模块中导入模型：

```Python hl_lines="4-5  10  12-14"
{!./docs_src/tutorial/code_structure/tutorial002/app.py!}
```

当然，所有的 `TYPE_CHECKING` 和类型注解字符串的技巧 **只需要在有循环导入的文件中** 使用。

因为 `app.py` 没有循环导入，我们可以直接使用正常的导入方式，并像平常一样使用类。

运行该程序将得到与之前相同的结果：

<div class="termy">

```console
$ python -m project.app

Created hero: id=1 age=None name='Deadpond' secret_name='Dive Wilson' team_id=1
Hero's team: id=1 name='Z-Force' headquarters='Sister Margaret's Bar'
```

</div>

## 总结

对于 **最简单的情况** （大多数情况），你可以将所有模型放在一个文件中，其他应用程序的结构（包括设置 **engine** ）可以分布在多个文件中。

而对于那些 **复杂的情况** ，需要将所有模型分离到不同的文件中时，可以使用 `TYPE_CHECKING` 来让所有内容正常工作，并保持最佳的开发体验和编辑器支持。✨
