# 使用 FastAPI 删除数据

现在，让我们添加一个 *路径操作* 来删除一个英雄。

这非常简单。😁

## 删除路径操作

因为我们要 **删除** 数据，所以使用 HTTP 的 `DELETE` 操作。

我们从路径参数中获取 `hero_id`，并验证它是否存在，正如我们在读取单个英雄或更新英雄时所做的那样，**可能会抛出一个 `404` 错误响应**。

如果确实找到该英雄，我们只需使用 **session** 将其删除。

//// tab | Python 3.10+

```Python hl_lines="3-11"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/delete/tutorial001_py310.py[ln:89-97]!}

# 代码省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3-11"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/delete/tutorial001_py39.py[ln:91-99]!}

# 代码省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-11"
# 代码省略 👆

{!./docs_src/tutorial/fastapi/delete/tutorial001.py[ln:91-99]!}

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

删除成功后，我们只需返回以下响应：

```JSON
{
    "ok": true
}
```

## 小结

就是这样，欢迎在交互式文档 UI 中尝试删除一些英雄。💥

使用 **FastAPI** 来读取数据并结合 **SQLModel** 使得从数据库中删除数据变得非常简单。