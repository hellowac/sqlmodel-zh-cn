# 使用 FastAPI 和 SQLModel 测试应用程序

为了完成这一组关于 **FastAPI** 和 **SQLModel** 的章节，我们现在来学习如何为使用 FastAPI 和 SQLModel 的应用程序实现自动化测试。✅

包括一些技巧和窍门。🎁

## FastAPI 应用程序

我们将使用我们在前几章中构建的一个 **简单** FastAPI 应用程序。

同样的 **概念**、**技巧** 和 **窍门** 也适用于更复杂的应用程序。

我们将使用包含英雄模型的应用程序，但不包括团队模型，并且我们将使用依赖项来获取一个 **会话**。

现在我们将看到拥有这个会话依赖项是多么有用。✨

/// details | 👀 完整文件预览

```Python
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/main.py!}
```

///

## 文件结构

现在我们将有一个包含多个文件的 Python 项目，一个文件 `main.py` 包含整个应用程序，一个文件 `test_main.py` 包含测试，遵循 [代码结构与多个文件](../code-structure.md){.internal-link target=_blank} 中的相同思路。

文件结构如下：

```
.
├── project
    ├── __init__.py
    ├── main.py
    └── test_main.py
```

## 测试 FastAPI 应用程序

如果你之前没有进行过 FastAPI 应用程序的测试，请首先查看 <a href="https://fastapi.tiangolo.com/tutorial/testing/" class="external-link" target="_blank">FastAPI 测试文档</a>。

然后，我们可以继续，这里的第一步是安装依赖项 `requests` 和 `pytest`。

确保你创建了一个 [虚拟环境](../../virtual-environments.md){.internal-link target=_blank}，并激活它，然后安装依赖项，例如使用：

<div class="termy">

```console
$ pip install requests pytest

---> 100%
```

</div>

## 基本测试代码

让我们从一个简单的测试开始，测试代码只是验证 **FastAPI** 应用程序是否能正确地创建一个新英雄。

```{ .python .annotate }
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_001.py[ln:1-7]!}
        # 这里省略了一些代码，我们稍后会看到 👈
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_001.py[ln:20-24]!}
        # 这里省略了一些代码，我们稍后会看到 👈
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_001.py[ln:26-32]!}

# 代码省略 👇
```

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/annotations/en/test_main_001.md!}

/// tip

查看代码行号气泡，了解每行代码的作用。

///

这就是我们稍后所有测试所需的 **核心** 代码。

但是现在，我们需要处理一些后勤工作和细节，之前我们还没有注意到这些。🤓

## 测试数据库

这个测试看起来没问题，但存在一个问题。

如果我们直接运行它，它将使用我们正在使用的 **生产数据库** 来存储我们非常重要的 **英雄** 数据，这样我们就可能向其中添加不必要的数据，甚至更糟，未来的测试中，我们可能会删除生产数据。

因此，我们应该使用一个独立的 **测试数据库**，仅供测试使用。

为此，我们需要更改用于数据库的 URL。

但是当 API 代码执行时，它会获取一个已经连接到 **引擎** 的 **会话**，而 **引擎** 已经使用了一个特定的数据库 URL。

即使我们从 `main` 模块导入变量并仅在测试中更改其值，到那时 **引擎** 已经使用原始值创建了。

但是我们所有的 API *路径操作* 都是通过 FastAPI **依赖项** 获取 *会话* 的，我们可以在测试中覆盖依赖项。

这就是依赖项开始大显身手的地方。

## 覆盖依赖项

我们将为测试覆盖 `get_session()` 依赖项。

这个依赖项被所有的 *路径操作* 用来获取 **SQLModel** 会话对象。

我们将覆盖它，使其仅在测试中使用一个不同的 **会话** 对象。

这样可以保护生产数据库，并更好地控制我们正在测试的数据。

```{ .python .annotate hl_lines="4  9-10  12  19" }
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_002.py[ln:1-7]!}
        # 这里省略了一些代码，我们稍后会看到 👈
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_002.py[ln:15-32]!}

# 代码省略 👇
```

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/annotations/en/test_main_002.md!}

/// tip

查看代码行号气泡，了解每行代码的作用。

///

## 为测试创建引擎和会话

现在让我们创建一个 **会话** 对象，供测试期间使用。

它将使用自己的 **引擎**，而这个新引擎将使用新的测试数据库 URL：

```
sqlite:///testing.db
```

所以，测试数据库将存储在 `testing.db` 文件中。

```{ .python .annotate hl_lines="4  8-11  13  16  33"}
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_003.py!}
```

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/annotations/en/test_main_003.md!}

### 导入表模型

在这里，我们使用以下代码创建测试数据库中的所有表：

```Python
SQLModel.metadata.create_all(engine)
```

但请记住，**顺序很重要**，[顺序很重要](../create-db-and-table.md#sqlmodel-metadata-order-matters){.internal-link target=_blank}，我们需要确保所有 **SQLModel** 模型都已经定义并 **导入**，然后再调用 `.create_all()`。

在这种情况下，它之所以有效，是因为我们导入了 `.main` 中的某些内容，*任何东西*，这将导致 `.main` 中的代码被执行，包括 **表模型** 的定义，这会自动将它们注册到 `SQLModel.metadata` 中。

这样，当我们调用 `.create_all()` 时，所有的 **表模型** 都会正确地注册到 `SQLModel.metadata` 中，一切都会正常工作。👌

## 内存数据库

现在我们不再使用生产数据库，而是使用了一个新的 **测试数据库**，存储在 `testing.db` 文件中，这很好。

但是，SQLite 也支持使用 **内存数据库**。这意味着整个数据库只存在于内存中，永远不会保存到磁盘上的文件中。

在程序终止后，**内存数据库会被删除**，因此对于生产数据库没有太大帮助。

但是，**它对测试非常有用**，因为它可以在每个测试之前快速创建，并在每个测试后快速删除。✅

而且，由于它永远不需要写入文件，一切都仅存在于内存中，它的速度会比通常的数据库更快。🏎

/// details | 其他替代方案和思路 👀

在考虑使用 **内存数据库** 之前，我们可以探索其他一些替代方案和思路。

首先，我们没有在测试结束后删除文件，因此下一个测试可能会有 **残留数据**。因此，正确的做法是在测试结束后立即删除文件。🔥

但是，如果每个测试都必须创建一个新文件，然后再删除它，运行所有测试可能会 **稍微慢一点**。

目前，我们有一个文件 `testing.db`，所有测试都使用这个文件（虽然现在只有一个测试，但我们将会有更多）。

因此，如果我们尝试同时 **并行** 运行测试以提高速度，它们可能会因为尝试使用 *相同的* `testing.db` 文件而发生冲突。

当然，我们也可以通过为每个测试数据库文件使用 **随机名称** 来解决这个问题……但对于 SQLite，我们有一个更好的替代方案——直接使用 **内存数据库**。✨

///

## 配置内存数据库

让我们更新代码，使用内存数据库。

我们只需要更改 **引擎** 中的几个参数。

```{ .python .annotate hl_lines="3  9-13"}
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_004.py[ln:1-13]!}

# 代码省略 👇
```

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/annotations/en/test_main_004.md!}

/// tip

查看代码行号气泡，了解每行代码的作用。

///

就这样，现在测试将使用 **内存数据库** 运行，这将更快，也可能更安全。

其他所有测试也可以使用相同的方法。

## 样板代码

很好，代码有效，你可以在每个测试函数中复制整个过程。

但我们不得不添加很多 **样板代码** 来处理自定义数据库，创建内存数据库、创建自定义会话和覆盖依赖项。

我们真的需要为 **每个测试** 都复制这些代码吗？不，我们可以做得更好！ 😎

我们使用 **pytest** 来运行测试。而且，pytest 也有一个与 **FastAPI** 依赖项非常相似的概念。

/// info

实际上，pytest 是启发 FastAPI 设计依赖项的因素之一。

///

它是一种让我们在每个测试之前声明一些 **代码** 并 **为测试函数提供一个值** 的方式（这几乎与 FastAPI 的依赖项相同）。

实际上，它也有类似的技巧，允许使用 `yield` 代替 `return` 来提供值，然后 **pytest** 会确保 `yield` 后的代码会在测试函数执行完后再执行。

在 pytest 中，这些东西叫做 **fixtures**，而不是 *依赖项*。

让我们使用这些 **fixtures** 来改进我们的代码，减少后续测试中的重复样板代码。

## Pytest Fixtures

你可以在 <a href="https://docs.pytest.org/en/6.2.x/fixture.html" class="external-link" target="_blank">pytest 文档中的 Fixtures</a> 中了解更多，但我会给你一个简短的示例，展示我们在这里需要的内容。

让我们看看第一个使用 fixture 的代码示例：

```{ .python .annotate }
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_005.py!}
```

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/annotations/en/test_main_005.md!}

/// tip

查看代码行号气泡，了解每行代码的作用。

///

**pytest** 的 fixtures 和 FastAPI 的依赖项工作方式非常相似，但有一些小的差别：

* 在 pytest fixtures 中，我们需要在上方添加 `@pytest.fixture()` 装饰器。
* 要在函数中使用 pytest fixture，我们必须声明参数的 **完全相同的名称**。在 FastAPI 中，我们必须显式地使用 `Depends()` 并将实际函数放在其中。

但除了声明方式和如何告知框架我们希望将其应用于函数的方式外，它们 **工作方式非常相似**。

现在，我们创建了许多测试并在其中重用相同的 fixture，从而节省了大量的 **样板代码**。

**pytest** 将确保在每个测试函数之前执行它们（并在之后执行它们）。所以，每个测试函数都会有自己独立的数据库、引擎和会话。

## 客户端 Fixture

太棒了，这个 fixture 帮助我们减少了大量的重复代码。

但目前，我们仍然需要在测试函数中编写一些重复的代码，目前我们需要：

* 创建 **依赖项覆盖**
* 将其放入 `app.dependency_overrides`
* 创建 `TestClient`
* 在发出请求后清理依赖项覆盖

这些在未来的其他测试中仍然是重复的。我们可以改进它吗？可以！🎉

每个 **pytest** fixture（和 **FastAPI** 依赖项一样），可以依赖其他 fixture。

因此，我们可以创建一个 **客户端 fixture**，它将在所有测试中使用，并且它本身需要 **会话 fixture**。

```{ .python .annotate hl_lines="19-28  31" }
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main_006.py!}
```

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/annotations/en/test_main_006.md!}

/// tip

查看代码行号气泡，了解每行代码的作用。

///

现在我们有了一个 **客户端 fixture**，它又依赖于 **会话 fixture**。

在实际的测试函数中，我们只需要声明需要这个 **客户端 fixture**。

## 添加更多测试

到目前为止，可能看起来我们做了很多更改，却没有得到任何新的结果，依然是 **相同的结果**。🤔

但通常情况下，我们会创建 **很多其他测试函数**。现在所有的样板代码和复杂性 **只写了一次**，就放在了这两个 fixture 中。

让我们添加更多的测试：

```Python hl_lines="3  22"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main.py[ln:30-58]!}

# 下面的代码省略 👇
```

/// details | 👀 完整文件预览

```Python
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main.py!}
```

///

/// tip

除了测试正常情况外，**测试无效数据**、**错误**和**边界情况**也是 **好主意**，确保它们能正确处理。

这就是我们在这里添加这两个额外测试的原因。

///

现在，任何额外的测试函数都可以像第一个测试一样简单，它们只需要 **声明 `client` 参数** 来获取已经设置好所有数据库内容的 `TestClient` **fixture**。很棒！😎

## 为什么是两个 Fixtures

现在，看到这些代码后，我们可能会想，为什么要使用 **两个 fixtures**，而不是 **只用一个** 包含所有代码的 fixture 呢？这个问题非常有道理！

对于这些示例，**用一个 fixture 更简单**，其实没必要把代码拆分成两个 fixture。

但对于下一个测试函数，我们将需要 **两个 fixture**，即 **client** 和 **session**。

```Python hl_lines="6  10"
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main.py[ln:1-6]!}

# 这里的代码省略 👈

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main.py[ln:61-81]!}

# 下面的代码省略 👇
```

/// details | 👀 完整文件预览

```Python
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main.py!}
```

///

在这个测试函数中，我们希望检查 **读取英雄列表** 的路径操作是否真的发送了英雄数据。

但如果 **数据库为空**，我们会得到一个 **空列表**，这时我们无法判断英雄数据是否正确发送。

但是我们可以在发送 API 请求之前 **在测试数据库中创建一些英雄**。✨

而且，由于我们使用的是 **测试数据库**，在测试中创建英雄数据不会影响其他内容。

为此，我们需要：

* 导入 `Hero` 模型
* 需要两个 fixtures，**client** 和 **session**
* 创建一些英雄并使用 **session** 将它们保存到数据库中

之后，我们就可以发送请求并检查是否从数据库中正确获取了数据。💯

这里需要注意的一个重要细节是：我们可以在其他 fixture **和** 测试函数中要求使用 fixtures。

**client fixture** 函数和实际的测试函数会 **都** 使用相同的 **session**。

## 添加其余的测试

利用相同的思路，要求 fixtures，创建测试所需的数据等等，我们现在可以添加剩余的测试。它们看起来与我们迄今为止所做的非常相似。

```Python hl_lines="3  18  33"
# 上面的代码省略 👆

{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main.py[ln:84-125]!}
```

/// details | 👀 完整文件预览

```Python
{!./docs_src/tutorial/fastapi/app_testing/tutorial001/test_main.py!}
```

///

## 运行测试

现在我们可以使用 `pytest` 运行测试，并查看结果：

<div class="termy">

```console
$ pytest

============= 测试会话开始 ==============
平台 linux -- Python 3.7.5, pytest-6.2.4, py-1.10.0, pluggy-0.13.1
根目录: /home/user/code/sqlmodel-tutorial
<b>已收集 7 项                              </b>

---> 100%

project/test_main.py <font color="#A6E22E">.......         [100%]</font>

<font color="#A6E22E">============== </font><font color="#A6E22E"><b>7 通过</b></font><font color="#A6E22E"> 0.83秒 ===============</font>
```

</div>

## 回顾

你都读完了吗？哇，真让我印象深刻！😎

为你的应用程序添加测试将为你提供很多 **确定性**，确保一切按预期 **正确工作**。

测试在 **重构** 代码、**更改内容**、**添加功能** 时尤其有用。因为测试能够帮助捕捉许多在重构时容易引入的错误。

它们还会让你更有信心地工作，**更高效**，因为你知道自己在检查 **没有破坏任何东西**。😅

我认为，测试是将你的代码和你作为开发者提升到下一个专业级别的东西之一。😎

如果你读完并研究了这一切，你已经掌握了许多高级的思想和技巧，而这些是我花了几年才学到的。🚀
