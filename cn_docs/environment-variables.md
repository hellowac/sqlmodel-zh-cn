# 环境变量

在我们进入代码之前，先了解一些 **基础知识**，帮助我们理解如何使用 Python（以及一般的编程）。让我们先看一下 **环境变量**。

/// tip

如果你已经知道什么是“环境变量”以及如何使用它们，可以跳过这一部分。

///

环境变量（也称为“**env var**”）是一个存在于 **操作系统** 中的变量，**不在** Python 代码内，但可以被 Python 代码（或者其他程序）读取。

环境变量对于处理应用程序的 **设置**、作为 Python 的 **安装** 一部分等非常有用。

## 创建和使用环境变量

你可以在 **终端（Shell）** 中 **创建** 和使用环境变量，无需 Python：

//// tab | Linux, macOS, Windows Bash

<div class="termy">

```console
// 你可以创建一个名为 MY_NAME 的环境变量
$ export MY_NAME="Wade Wilson"

// 然后你可以在其他程序中使用它，比如：
$ echo "Hello $MY_NAME"

Hello Wade Wilson
```

</div>

////

//// tab | Windows PowerShell

<div class="termy">

```console
// 创建一个环境变量 MY_NAME
$ $Env:MY_NAME = "Wade Wilson"

// 在其他程序中使用它，比如：
$ echo "Hello $Env:MY_NAME"

Hello Wade Wilson
```

</div>

////

## 在 Python 中读取环境变量

你也可以在 **Python 外部** 创建环境变量（通过终端或其他方法），然后 **在 Python 中读取它们**。

例如，你可以有一个文件 `main.py`，其中包含：

```Python hl_lines="3"
import os

name = os.getenv("MY_NAME", "World")
print(f"Hello {name} from Python")
```

/// tip

`os.getenv()` 的第二个参数是默认值。如果没有提供，默认为 `None`，在这里我们提供 `"World"` 作为默认值。

///

然后你可以调用这个 Python 程序：

//// tab | Linux, macOS, Windows Bash

<div class="termy">

```console
// 这里我们还没有设置环境变量
$ python main.py

// 因为我们没有设置环境变量，所以得到默认值

Hello World from Python

// 但是，如果我们先创建环境变量
$ export MY_NAME="Wade Wilson"

// 然后再次调用程序
$ python main.py

// 现在它可以读取环境变量

Hello Wade Wilson from Python
```

</div>

////

//// tab | Windows PowerShell

<div class="termy">

```console
// 这里我们还没有设置环境变量
$ python main.py

// 因为我们没有设置环境变量，所以得到默认值

Hello World from Python

// 但是，如果我们先创建环境变量
$ $Env:MY_NAME = "Wade Wilson"

// 然后再次调用程序
$ python main.py

// 现在它可以读取环境变量

Hello Wade Wilson from Python
```

</div>

////

由于环境变量可以在代码之外设置，但可以被代码读取，而且不需要和其他文件一起存储（提交到 `git`），因此通常用它们来配置或存储 **设置**。

你还可以为 **特定的程序调用** 创建环境变量，它仅对该程序有效，并且只在程序运行期间存在。

为此，可以在程序本身之前的同一行创建环境变量：

<div class="termy">

```console
// 在这一行中为该程序调用创建环境变量 MY_NAME
$ MY_NAME="Wade Wilson" python main.py

// 现在它可以读取环境变量

Hello Wade Wilson from Python

// 程序调用后环境变量不再存在
$ python main.py

Hello World from Python
```

</div>

/// tip

你可以在 <a href="https://12factor.net/config" class="external-link" target="_blank">《The Twelve-Factor App: Config》</a> 中阅读更多相关内容。

///

## 类型与验证

这些环境变量只能处理 **文本字符串**，因为它们在 Python 之外，必须与其他程序和整个系统兼容（甚至与不同操作系统如 Linux、Windows、macOS 兼容）。

这意味着 **从环境变量读取的任何值** 都将是一个 `str` 类型，任何转换为其他类型或验证都必须在代码中进行。

## `PATH` 环境变量

有一个 **特殊的** 环境变量叫做 **`PATH`**，操作系统（Linux、macOS、Windows）使用它来查找程序并执行。

`PATH` 变量的值是一个长字符串，由多个目录组成，Linux 和 macOS 用冒号 `:` 分隔，Windows 用分号 `;` 分隔。

例如，`PATH` 环境变量可能是这样的：

//// tab | Linux, macOS

```plaintext
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

这意味着系统应该在以下目录中查找程序：

* `/usr/local/bin`
* `/usr/bin`
* `/bin`
* `/usr/sbin`
* `/sbin`

////

//// tab | Windows

```plaintext
C:\Program Files\Python312\Scripts;C:\Program Files\Python312;C:\Windows\System32
```

这意味着系统应该在以下目录中查找程序：

* `C:\Program Files\Python312\Scripts`
* `C:\Program Files\Python312`
* `C:\Windows\System32`

////

当你在终端中输入 **命令** 时，操作系统会 **在 `PATH` 环境变量中列出的每个目录中查找** 程序。

例如，当你在终端中输入 `python` 时，操作系统会首先在 `PATH` 中的 **第一个目录** 查找名为 `python` 的程序。

如果找到，它就会 **使用** 该程序。如果没有找到，它会继续在 **其他目录** 中查找。

### 安装 Python 和更新 `PATH`

当你安装 Python 时，可能会被问到是否要更新 `PATH` 环境变量。

//// tab | Linux, macOS

假设你安装了 Python，安装目录是 `/opt/custompython/bin`。

如果你选择更新 `PATH` 环境变量，安装程序将会把 `/opt/custompython/bin` 添加到 `PATH` 环境变量中。

它可能看起来像这样：

```plaintext
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/custompython/bin
```

这样，当你在终端中输入 `python` 时，系统就会在 `/opt/custompython/bin`（最后一个目录）中找到 Python 程序，并使用这个版本。

////

//// tab | Windows

假设你安装了 Python，安装目录是 `C:\opt\custompython\bin`。

如果你选择更新 `PATH` 环境变量，安装程序将会把 `C:\opt\custompython\bin` 添加到 `PATH` 环境变量中。

```plaintext
C:\Program Files\Python312\Scripts;C:\Program Files\Python312;C:\Windows\System32;C:\opt\custompython\bin
```

这样，当你在终端中输入 `python` 时，系统就会在 `C:\opt\custompython\bin`（最后一个目录）中找到 Python 程序，并使用这个版本。

////

这样，当你在终端中输入 `python` 时，系统就会在 `/opt/custompython/bin`（最后一个目录）中找到 Python 程序，并使用它。

因此，如果你输入：

<div class="termy">

```console
$ python
```

</div>

//// tab | Linux, macOS

系统将会 **找到** `/opt/custompython/bin` 中的 `python` 程序并执行它。

这大致相当于输入：

<div class="termy">

```console
$ /opt/custompython/bin/python
```

</div>

////

//// tab | Windows

系统将会 **找到** `C:\opt\custompython\bin\python` 中的 `python` 程序并执行它。

这大致相当于输入：

<div class="termy">

```console
$ C:\opt\custompython\bin\python
```

</div>

////

这些信息将在学习 [虚拟环境](virtual-environments.md) 时派上用场。

## 结论

通过这些内容，你应该对 **环境变量** 有了基本的理解，并知道如何在 Python 中使用它们。

你也可以在 [维基百科](https://en.wikipedia.org/wiki/Environment_variable) 上阅读更多有关环境变量的信息。

在许多情况下，环境变量的用途可能不会立即显现出来，但它们会在开发过程中出现在许多不同的场景中，因此了解它们是很有用的。

例如，你将在下一节 [虚拟环境](virtual-environments.md) 中需要用到这些信息。
