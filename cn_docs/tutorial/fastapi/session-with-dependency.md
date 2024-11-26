# 使用 FastAPI 依赖项管理 Session

在我们继续添加功能之前，让我们稍微改变一下获取每个请求的 session 的方式，以便以后简化我们的工作。

## 当前的 Session

到目前为止，我们在每个 *路径操作* 中都在 `with` 块中创建了一个 session。

//// tab | Python 3.10+

```Python hl_lines="5"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/delete/tutorial001_py310.py[ln:48-55]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="5"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/delete/tutorial001_py39.py[ln:50-57]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="5"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/delete/tutorial001.py[ln:50-57]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/delete/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/delete/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/delete/tutorial001.py!}
```

////

///

这样做是完全没问题的，但在许多情况下，我们希望使用 [FastAPI 依赖项](https://fastapi.tiangolo.com/tutorial/dependencies/)，例如在执行任何其他代码之前， **验证** 客户端是否 **登录** 并获取 **当前用户** 。

这些依赖项在 **测试** 中也非常有用，因为我们可以 **轻松地替换它们**，然后例如为测试使用新的数据库，或在测试前插入一些数据等。

因此，让我们重构这些 session，使其使用 **FastAPI 依赖项**。

## 创建一个 **FastAPI** 依赖项

**FastAPI** 依赖项非常简单，它只是一个返回值的函数。

它可以使用 `yield` 替代 `return`，在这种情况下，**FastAPI** 会确保在请求完成后执行所有 `yield` 之后的代码。

//// tab | Python 3.10+

```Python hl_lines="3-5"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:40-42]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3-5"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:42-44]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-5"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:42-44]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py!}
```

////

///

## 使用依赖项

现在，让我们让 FastAPI 执行一个依赖项，并在 *路径操作* 中获取它的值。

我们从 `fastapi` 导入 `Depends()`，然后在 *路径操作函数* 中使用它作为一个 **参数**，就像我们声明参数以获取 JSON 数据体、路径参数等一样。

//// tab | Python 3.10+

```Python hl_lines="1  13"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:1-2]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:40-42]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:53-59]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3  15"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:55-61]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3  15"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:55-61]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py!}
```

////

///

/// tip

这里有一个关于参数中 `*,` 的小贴士。

在这里，我们传递了一个参数 `session`，它的“默认值”是 `Depends(get_session)`，位于没有默认值的参数 `hero` 之前。

通常情况下，Python 会抱怨这个，但我们可以使用初始的“参数” `*,` 来将剩下的参数标记为“仅限关键字”，这样就解决了这个问题。

你可以在 FastAPI 文档中阅读更多关于这个的内容：[路径参数和数字验证 - 按需排序参数技巧](https://fastapi.tiangolo.com/tutorial/path-params-numeric-validations/#order-the-parameters-as-you-need-tricks)

///

依赖项的值 **只会用于一次请求**，FastAPI 会在调用你的代码之前调用它，并将依赖项的值传递给你。

如果它有 `yield`，那么在你完成发送响应后，FastAPI 会继续执行 `yield` 之后的其余代码。在 **session** 的情况下，它会完成 `with` 块中的清理代码，关闭 session 等。

然后，FastAPI 会在 **下一个请求** 时再次调用它。

由于它是 **每次请求调用一次**，我们仍然会为每个请求获取 **单个 session**，因此我们仍然是可以接受的。✅

因为依赖项可以使用 `yield`，FastAPI 会确保在完成后运行 `yield` 之后的代码，包括 `with` 块末尾的所有 **清理代码**。所以我们也可以接受这一点。✅

## `with` 块

这意味着在 *路径操作函数* 的主代码中，它将与之前使用显式 `with` 块的版本等效。

//// tab | Python 3.10+

```Python hl_lines="14-18"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:1-2]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:40-42]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:53-59]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="16-20"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:55-61]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="16-20"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:55-61]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py!}
```

////

///

实际上，你可以认为 `create_hero()` 函数中所有的代码块仍然是在 **session** 的 `with` 块内，因为这基本上是幕后发生的事情。

但现在，`with` 块并不显式在函数内，而是在上面的依赖项中：

//// tab | Python 3.10+

```Python hl_lines="7-8"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:1-2]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:40-42]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:53-59]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9-10"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:55-61]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9-10"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:55-61]!}

# 代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py!}
```

////

///

我们稍后会看到，这在测试代码时非常有用。✅

## 更新路径操作以使用依赖项

现在，我们可以更新其余的 *路径操作* 以使用新的依赖项。

我们只需在函数的参数中声明依赖项，使用：

```Python
session: Session = Depends(get_session)
```

然后，移除之前使用旧 **session** 的 `with` 块。

//// tab | Python 3.10+

```Python hl_lines="13  24  33  42  57"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:1-2]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:40-42]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py[ln:53-104]!}
```

////

//// tab | Python 3.9+

```Python hl_lines="15  26  35  44  59"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py[ln:55-106]!}
```

////

//// tab | Python 3.7+

```Python hl_lines="15  26  35  44  59"
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:1-4]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:42-44]!}

# 代码省略 👈

{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py[ln:55-106]!}
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/session_with_dependency/tutorial001.py!}
```

////

///

## 小结

你刚刚学习了如何使用 **FastAPI 依赖项** 来处理数据库会话。这将在稍后的测试代码中派上用场。

随着你在 FastAPI 中的工作越来越多，你会发现这些依赖项能帮助你处理 **权限**、**认证**、数据库 **会话** 等资源。🚀

如果你想了解更多关于依赖项的内容，请查看 <a href="https://fastapi.tiangolo.com/tutorial/dependencies/" class="external-link" target="_blank">FastAPI 文档中的依赖项</a>。
