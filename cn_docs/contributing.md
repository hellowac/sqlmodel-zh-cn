# 贡献

首先，你可能想查看一些基本的方式来 [帮助 SQLModel 并获取帮助](help.md){.internal-link target=_blank}。

## 开发

如果你已经克隆了 <a href="https://github.com/fastapi/sqlmodel" class="external-link" target="_blank">sqlmodel 仓库</a>，并且想深入研究代码，以下是一些设置环境的指南。

### 虚拟环境

按照说明创建并激活一个 [虚拟环境](virtual-environments.md){.internal-link target=_blank}，用于 `sqlmodel` 的内部代码。

### 使用 `pip` 安装依赖

激活环境后，安装所需的包：

<div class="termy">

```console
$ pip install -r requirements.txt

---> 100%
```

</div>

这将安装所有依赖项并在本地环境中安装你的 SQLModel 本地版本。

### 使用本地 SQLModel

如果你创建一个 Python 文件，导入并使用 SQLModel，然后用你本地环境中的 Python 运行它，它将使用你克隆的本地 SQLModel 源代码。

当你更新本地 SQLModel 源代码时，再次运行 Python 文件时，它将使用你刚刚编辑的最新版本的 SQLModel。

这样，你不必“安装”本地版本就能测试每次更改。

/// note | “技术细节”

只有在使用这个包含的 `requirements.txt` 文件安装时才会发生这种情况，而不是直接运行 `pip install sqlmodel`。

这是因为在 `requirements.txt` 文件中，本地版本的 SQLModel 被标记为“可编辑”模式，使用了 `-e` 选项。

///

### 格式化

你可以运行一个脚本，格式化并清理你的所有代码：

<div class="termy">

```console
$ bash scripts/format.sh
```

</div>

它还会自动对所有导入进行排序。

## 测试

你可以运行一个脚本，在本地测试所有代码并生成 HTML 格式的覆盖率报告：

<div class="termy">

```console
$ bash scripts/test-cov-html.sh
```

</div>

此命令会生成一个 `./htmlcov/` 目录，如果你在浏览器中打开 `./htmlcov/index.html` 文件，你可以互动地查看被测试覆盖的代码区域，并注意是否有任何区域没有被覆盖。

## 文档

首先，确保你按照上面的描述设置好环境，这将安装所有的依赖。

### 文档实时更新

在本地开发期间，有一个脚本可以构建网站并检查任何变化，支持实时重载：

<div class="termy">

```console
$ python ./scripts/docs.py live

<span style="color: green;">[INFO]</span> Serving on http://127.0.0.1:8008
<span style="color: green;">[INFO]</span> Start watching changes
<span style="color: green;">[INFO]</span> Start detecting changes
```

</div>

它会在 `http://127.0.0.1:8008` 上提供文档服务。

这样，你可以编辑文档/源文件并实时查看变化。

/// tip

或者，你可以手动执行脚本中的相同步骤。

进入 `docs/` 目录：

```console
$ cd docs/
```

然后在该目录下运行 `mkdocs`：

```console
$ mkdocs serve --dev-addr 8008
```

///

#### Typer CLI（可选）

此处的说明展示了如何直接使用 `python` 程序运行 `./scripts/docs.py` 脚本。

但你也可以使用 <a href="https://typer.tiangolo.com/typer-cli/" class="external-link" target="_blank">Typer CLI</a>，并在终端中为命令提供自动补全功能，前提是已安装补全功能。

如果你安装了 Typer CLI，可以通过以下命令安装补全功能：

<div class="termy">

```console
$ typer --install-completion

zsh completion installed in /home/user/.bashrc.
Completion will take effect once you restart the terminal.
```

</div>

### 文档结构

文档使用了 <a href="https://www.mkdocs.org/" class="external-link" target="_blank">MkDocs</a>。

并且在 `./scripts/docs.py` 中有一些额外的工具/脚本。

/// tip

你不需要查看 `./scripts/docs.py` 中的代码，只需在命令行中使用它。

///

所有文档都存储在 `./docs` 目录下，并使用 Markdown 格式。

许多教程中有代码块。

在大多数情况下，这些代码块是完整的应用程序，可以直接运行。

实际上，这些代码块并不是写在 Markdown 文件中，而是存储在 `./docs_src/` 目录中的 Python 文件。

这些 Python 文件在生成网站时会被包含/注入到文档中。

### 测试文档

大多数测试实际上是针对文档中的示例源文件进行的。

这有助于确保：

* 文档是最新的。
* 文档中的示例代码可以直接运行。
* 大多数功能都被文档覆盖，通过测试覆盖率得以确保。
