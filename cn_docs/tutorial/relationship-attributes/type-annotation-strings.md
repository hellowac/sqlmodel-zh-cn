# 类型注解字符串

## 关于 `List["Hero"]` 中的字符串

在第一个 `Relationship` 属性中，我们使用 `List["Hero"]` 来声明它，而不是直接写 `List[Hero]`，将 `Hero` 放在引号中：

//// tab | Python 3.10+

```Python hl_lines="9"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py310.py[ln:1-19]!}

# Code below omitted 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py39.py[ln:1-21]!}

# Code below omitted 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11"
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001.py[ln:1-21]!}

# Code below omitted 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/define_relationship_attributes/tutorial001.py!}
```

////

///

这是怎么回事？我们不能像正常那样写 `List[Hero]` 吗？

在那行代码执行时，Python 解释器 **并不知道 `Hero` 这个类**，如果我们直接写它，它会尝试查找但失败，进而抛出错误。😭

但是通过把它放在引号里，作为一个字符串，解释器会把它看作是一个包含 `"Hero"` 的字符串。

而编辑器和其他工具可以看到 **这个字符串实际上是一个类型注解**，并提供自动补全、类型检查等功能。🎉

当然，**SQLModel** 也能正确理解这个字符串。✨

这其实是 Python 的一部分，它是当前官方解决方案，用来处理这个问题。

/// info

Python 本身正在进行大量工作，简化这一过程并让其更加直观，并寻找方法使得不需要将类包装在字符串中。

///
