# 读取关系

现在我们已经知道如何使用 **关系属性** 来连接数据，让我们来看一下如何从关系中获取和读取对象。

## 选择一个英雄

首先，添加一个 `select_heroes()` 函数，用来获取一个英雄对象并开始操作，并将该函数添加到 `main()` 函数中：

//// tab | Python 3.10+

```Python hl_lines="3-7  14"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py[ln:94-98]!}

# 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py[ln:108-111]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="3-7  14"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py[ln:96-100]!}

# 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py[ln:110-113]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="3-7  14"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py[ln:96-100]!}

# 之前的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py[ln:110-113]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py!}
```

////

///

## 选择关联的团队 - 传统方法

现在我们已经获取了一个英雄，我们可以获得这个英雄所属的团队。

按照到目前为止我们学到的内容，我们可以使用 `select()` 语句，然后通过 `session.exec()` 执行它，最后获取 `.first()` 结果，例如：

//// tab | Python 3.10+

```Python hl_lines="9-12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py[ln:94-103]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9-12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py[ln:96-105]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9-12"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py[ln:96-105]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py!}
```

////

///

## 获取关系团队 - 新方式

但现在我们有了 **关系属性**，我们可以直接访问它们，**SQLModel**（实际上是 SQLAlchemy）将自动从数据库中获取相应的数据，并将其加载到该属性中。✨

因此，上面高亮显示的代码块，与下面的代码块效果相同：

//// tab | Python 3.10+

```Python hl_lines="11"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py[ln:94-98]!}

        # 之前示例的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py[ln:105]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="11"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py[ln:96-100]!}

        # 之前示例的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py[ln:107]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="11"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py[ln:96-100]!}

        # 之前示例的代码省略 👈

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py[ln:107]!}

# 代码下面部分省略 👇
```

////

/// details | 👀 完整文件预览

//// tab | Python 3.10+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001_py39.py!}
```

////

//// tab | Python 3.7+

```Python
{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial001.py!}
```

////

///

/// tip

只要起始对象（在这个例子中是 `Hero`）与 **开放** 的会话关联，自动数据获取就会生效。

例如，在这里，**在** `Session` 对象的 `with` 代码块内。

///

## 获取关系对象列表

同样地，当我们在 **一对多** 关系的 **多** 端工作时，只需访问关系属性，就可以获取相关对象的列表：

//// tab | Python 3.10+

```Python hl_lines="9"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py310.py[ln:94-100]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.9+

```Python hl_lines="9"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002_py39.py[ln:96-102]!}

# 代码下面部分省略 👇
```

////

//// tab | Python 3.7+

```Python hl_lines="9"
# 代码上面部分省略 👆

{!./docs_src/tutorial/relationship_attributes/read_relationships/tutorial002.py[ln:96-102]!}

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

这将打印出所有属于 **Preventers** 团队的英雄列表：

<div class="termy">

```console
$ python app.py

// 自动获取英雄数据
INFO Engine SELECT hero.id AS hero_id, hero.name AS hero_name, hero.secret_name AS hero_secret_name, hero.age AS hero_age, hero.team_id AS hero_team_id
FROM hero
WHERE ? = hero.team_id
INFO Engine [cached since 0.8774s ago] (2,)

// 打印 Preventers 团队的英雄列表
Preventers heroes: [
    Hero(name='Rusty-Man', age=48, id=2, secret_name='Tommy Sharp', team_id=2),
    Hero(name='Spider-Boy', age=None, id=3, secret_name='Pedro Parqueador', team_id=2),
    Hero(name='Tarantula', age=32, id=6, secret_name='Natalia Roman-on', team_id=2),
    Hero(name='Dr. Weird', age=36, id=7, secret_name='Steve Weird', team_id=2),
    Hero(name='Captain North America', age=93, id=8, secret_name='Esteban Rogelios', team_id=2)
]
```

</div>

## 小结

通过 **关系属性**，你可以利用常见的 Python 对象轻松访问数据库中的相关数据。😎
