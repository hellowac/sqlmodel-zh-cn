# 安装 **SQLModel**

创建一个项目目录，创建一个 [虚拟环境](virtual-environments.md){.internal-link target=_blank}，激活它，然后安装 **SQLModel**，例如使用以下命令：

<div class="termy">

```console
$ pip install sqlmodel
---> 100%
Successfully installed sqlmodel pydantic sqlalchemy
```

</div>

由于 **SQLModel** 是建立在 <a href="https://www.sqlalchemy.org/" class="external-link" target="_blank">SQLAlchemy</a> 和 <a href="https://pydantic-docs.helpmanual.io/" class="external-link" target="_blank">Pydantic</a> 之上的，当你安装 `sqlmodel` 时，它们也会被自动安装。

## 安装 SQLite 数据库浏览器

记得 [SQLite 是一个单文件数据库](databases.md#a-single-file-database){.internal-link target=_blank} 吗？

在本教程的大多数示例中，我将使用 SQLite。

Python 集成了对 SQLite 的支持，它是一个单一的文件，可以直接从 Python 中读取和处理。而且它不需要 [外部数据库服务器](databases.md#a-server-database){.internal-link target=_blank}，所以它非常适合用于学习。

事实上，SQLite 完全可以处理相当大的应用程序。某些时候，你可能会想迁移到基于服务器的数据库，比如 <a href="https://www.postgresql.org/" class="external-link" target="_blank">PostgreSQL</a>（它也是免费的）。但现在我们会继续使用 SQLite。

在教程中，我会展示 SQL 片段和 Python 示例。我希望（并且预期 🧐）你能够实际运行它们，并验证数据库是否按预期工作并显示相同的数据。

为了能够独立于 Python 代码（并且可能同时）自己浏览 SQLite 文件，我推荐你使用 <a href="https://sqlitebrowser.org/" class="external-link" target="_blank">DB Browser for SQLite</a>。

它是一个很棒且简单的程序，可以通过友好的用户界面与 SQLite 数据库（SQLite 文件）进行交互。

<img src="https://sqlitebrowser.org/images/screenshot.png">

继续前往并 <a href="https://sqlitebrowser.org/" class="external-link" target="_blank">安装 DB Browser for SQLite</a>，它是免费的。

## 下一步

好了，开始吧！在接下来的章节中，我们将开始 [教程 - 用户指南](tutorial/index.md)。 🚀
