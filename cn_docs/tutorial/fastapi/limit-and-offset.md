# 使用 FastAPI 获取带限制和偏移量的英雄列表

当客户端发送请求获取所有英雄时，我们之前返回的是所有英雄。

但是，如果我们有 **成千上万** 的英雄，这可能会消耗大量的 **计算资源**、网络带宽等。

因此，我们可能希望对此进行限制。

让我们使用前面教程章节中学到的 **偏移量**（offset）和 **限制**（limit）来处理 API 请求。

/// info

在许多情况下，这也叫做 **分页**。

///

## 将限制和偏移量添加到查询参数

我们将 `limit` 和 `offset` 添加到查询参数中。

默认情况下，我们将返回数据库中的前几条结果，因此 `offset` 的默认值为 `0`。

默认情况下，我们将返回最多 `100` 个英雄，因此 `limit` 的默认值为 `100`。

//// tab | Python 3.10+

```Python hl_lines="1  7  9"
{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001_py310.py[ln:1-2]!}

# 这里的代码省略 👈

{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001_py310.py[ln:52-56]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3  9  11"
{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001_py39.py[ln:1-4]!}

# 这里的代码省略 👈

{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001_py39.py[ln:54-58]!}

# 下面的代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3  9  11"
{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001.py[ln:1-4]!}

# 这里的代码省略 👈

{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001.py[ln:54-58]!}

# 下面的代码省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/fastapi/limit_and_offset/tutorial001.py!}
```

////

///

我们希望允许客户端设置不同的 `offset` 和 `limit` 值。

但我们不希望他们设置一个像 `9999` 这样的 `limit`，那简直太多了！ 😱

所以，为了防止这种情况，我们在 `limit` 查询参数中添加了额外的验证，声明它必须小于或等于 `100`，使用 `le=100`。

这样，客户端可以决定获取更少的英雄，但不能超过这个限制。

/// info

如果你需要刷新查询参数及其验证的工作原理，可以查看 FastAPI 文档：

* <a href="https://fastapi.tiangolo.com/tutorial/query-params/" class="external-link" target="_blank">查询参数</a>
* <a href="https://fastapi.tiangolo.com/tutorial/query-params-str-validations/" class="external-link" target="_blank">查询参数和字符串验证</a>
* <a href="https://fastapi.tiangolo.com/tutorial/path-params-numeric-validations/" class="external-link" target="_blank">路径参数和数值验证</a>

///

## 查看文档 UI

现在，我们可以看到文档 UI 显示了新的参数，用于控制数据的 **limit** 和 **offset**。

<img class="shadow" alt="Interactive API docs UI" src="/img/tutorial/fastapi/limit-and-offset/image01.png">

## 总结

你可以使用 **FastAPI** 的自动数据验证来获取 `limit` 和 `offset` 的参数，然后使用它们与 **session** 一起控制响应中发送的数据范围。
