# 移除关系

现在假设 **Spider-Boy** 对 **Rusty-Man** 说：

> 我感觉不太好，Sharp先生

然后由于某些原因，他需要离开 **Preventers** 团队几年。😭

我们可以通过将关系设置为 `None` 来移除关系，和设置 `team_id` 一样，这也适用于新的关系属性 `.team`：

//// tab | Python 3.10+

```Python hl_lines="9"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py310.py[ln:103-114]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py39.py[ln:105-116]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002.py[ln:105-116]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002.py!}
```

////

///

当然，我们还需要记得将 `update_heroes()` 函数添加到 `main()` 中，以便在我们从命令行调用这个程序时运行：

//// tab | Python 3.10+

```Python hl_lines="7"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py310.py[ln:117-121]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="7"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py39.py[ln:119-123]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="7"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002.py[ln:119-123]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002.py!}
```

////

///

## 小结

这一章实在是太简短了，不是吗？🤔

无论如何，**关系属性** 使得处理存储在数据库中的关系变得既简单又直观。🎉