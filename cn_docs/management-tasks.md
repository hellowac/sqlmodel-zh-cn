# 仓库管理任务

以下是团队成员可以执行的管理 SQLModel 仓库的任务【参见团队成员说明](./management.md#team){.internal-link target=_blank}。

/// tip

本节内容仅对少数具有管理仓库权限的团队成员有用，其他人可以跳过。😉

///

...所以，你是 SQLModel 的 [团队成员](./management.md#team){.internal-link target=_blank}？哇，太酷了！ 😎

你可以像外部贡献者一样，通过 [帮助 SQLModel - 获取帮助](./help.md){.internal-link target=_blank} 来协助一切。此外，还有一些任务只有你（作为团队成员）可以执行。

以下是你可以执行任务的一般说明。

非常感谢你的帮助。🙇

## 做个友善的人

首先，要友善。😊

如果你已经被加入团队，那你大概是个超级友善的人，但还是值得提一下。🤓

### 当事情变得困难时

当一切顺利时，事情就容易多了，所以不需要太多说明。但当事情变得困难时，以下是一些指导原则。

尽量找出积极的一面。一般来说，如果别人没有表现得不友好，尽量感谢他们的努力和兴趣，即使你不同意讨论的主题（例如讨论或 PR），也要感谢他们对项目的兴趣，或者感谢他们花时间尝试做某事。

用文字表达情感是很困难的，别忘了使用表情符号来帮助表达。😅

在讨论和 PR 中，很多时候人们会带着情绪发言，有时不加过滤地表达沮丧，夸张抱怨、发泄、不合适的要求等等。这些都不太友好，发生这种情况时，我们会优先考虑解决他们的问题。但仍然要深呼吸，尽量温和地回应。

尽量避免使用尖刻的讽刺或可能带有消极攻击性的评论。如果有什么不对，最好直接指出来（尽量温和），而不是讽刺。

尽量具体和客观，避免泛泛而谈。

对于更棘手的对话，例如拒绝 PR，你可以让我（@tiangolo）直接处理。

## 编辑 PR 标题

* 编辑 PR 标题时，以来自 <a href="https://gitmoji.dev/" class="external-link" target="_blank">gitmoji</a> 的表情符号开头。
    * 使用表情符号字符，而不是 GitHub 代码。所以使用 `🐛` 而不是 `:bug:`。这样在 GitHub 以外的地方，例如发布说明中，也能正确显示。
* 标题以动词开头。例如 `Add`（添加），`Refactor`（重构），`Fix`（修复）等。这样标题会表达 PR 做了什么操作。例如 `Add support for teleporting`（添加对传送的支持），而不是 `Teleporting wasn't working, so this PR fixes it`（传送无法使用，所以这个 PR 修复了它）。
* 编辑 PR 标题时，使用“命令式”语气，像是给出命令。因此，使用 `Add support for teleporting` 而不是 `Adding support for teleporting`。
* 尽量让标题具体描述它实现的功能。如果是新功能，尝试描述它，例如 `Add support for teleporting`（添加对传送的支持），而不是 `Create TeleportAdapter class`（创建 TeleportAdapter 类）。
* 标题不要以句号（`.`）结尾。

一旦 PR 被合并，GitHub Action（<a href="https://github.com/tiangolo/latest-changes" class="external-link" target="_blank">latest-changes</a>）会使用 PR 标题自动更新最新的变更。

因此，拥有一个漂亮的 PR 标题不仅在 GitHub 中看起来很棒，而且在发布说明中也会很漂亮。 📝

## 为 PR 添加标签

相同的 GitHub Action <a href="https://github.com/tiangolo/latest-changes" class="external-link" target="_blank">latest-changes</a> 会使用 PR 中的一个标签来决定将该 PR 放入发布说明中的哪个部分。

确保使用来自 <a href="https://github.com/tiangolo/latest-changes#using-labels" class="external-link" target="_blank">latest-changes 标签列表</a> 中的支持标签：

* `breaking`：重大更改
    * 如果用户更新版本后代码会出错，需要修改代码才能继续使用的情况。这种情况发生的很少，所以这个标签不常用。
* `security`：安全修复
    * 这是针对安全修复的标签，例如漏洞修复。几乎不会使用。
* `feature`：新功能
    * 新增的功能，支持以前没有的内容。
* `bug`：修复
    * 以前支持的功能无法正常工作，通过此 PR 修复了它。有许多 PR 声称是 bug 修复，因为用户以不支持的方式使用它，并认为这应该是默认支持的。但在某些情况下，确实存在 bug。
* `refactor`：重构
    * 通常是对内部代码的修改，不会改变行为。通常是为了提高可维护性或为未来的功能铺路等。
* `upgrade`：升级
    * 这是针对项目的直接依赖项的升级，或额外的可选依赖项，通常位于 `pyproject.toml` 中。因此，最终用户更新后会收到这些升级，但不包括开发、测试、文档等方面的内部依赖项。这些内部依赖项通常在 `requirements.txt` 文件或 GitHub Actions 版本中，应该标记为 `internal`，而不是 `upgrade`。
* `docs`：文档
    * 文档的更改，包括更新文档、修复错别字。但不包括翻译的更改。
    * 通常可以通过访问 PR 中的 "Files changed" 标签，检查是否更新的文件以 `docs/en/docs` 开头来快速识别。文档的原始版本始终是英文的，位于 `docs/en/docs` 中。
* `internal`：内部
    * 用于只影响仓库管理的更改。例如，升级内部依赖项、更改 GitHub Actions 或脚本等。

/// tip

像 Dependabot 等工具会添加一些标签，比如 `dependencies`，但请注意，这些标签不会被 `latest-changes` GitHub Action 使用，因此不会出现在发布说明中。请确保添加上述标签之一。

///

## 审核 PR

如果 PR 没有解释它做了什么或者为什么这么做，请要求提供更多信息。

PR 应该有一个特定的用例，解释它解决了什么问题。

* 如果 PR 是新功能，它应该有文档。
    * 除非这是我们想要避免的功能，比如支持某个不常见的用例，不希望用户使用的功能。
* 文档应该包含源代码示例文件，而不是直接在 Markdown 中写 Python 代码。
* 如果源代码示例文件对于 Python 3.8、3.9 和 3.10 有不同的语法，应该提供不同版本的文件，并在文档中使用标签页展示它们。
* 应该有测试覆盖源代码示例。
* 在 PR 被应用之前，新测试应该是失败的。
* 应用 PR 后，新测试应该通过。
* 覆盖率应该保持在 100%。
* 如果你认为 PR 没问题，或者我们讨论后认为应该接受它，可以在 PR 上添加提交来调整它，增加文档、测试、格式、重构、删除多余文件等。
* 欢迎在 PR 中评论，要求提供更多信息、建议更改等。
* 一旦你认为 PR 准备好了，将它移动到 GitHub 内部项目中让我来审核。

## Dependabot PRs

Dependabot 会创建 PR 来更新多个依赖项，这些 PR 看起来相似，但有些要比其他的更微妙。

* 如果 PR 是针对直接依赖项的，即 Dependabot 正在修改 `pyproject.toml`，**不要合并它**。 😱 让我先检查一下。很可能需要进行一些额外的调整或更新。
* 如果 PR 更新了某个内部依赖项，例如修改了 `requirements.txt` 文件或 GitHub Action 版本，如果测试通过，发布说明（PR 中的摘要）没有明显的潜在破坏性更改，你可以合并它。 😎

## 标记 GitHub Discussions 答复

当 GitHub Discussions 中的问题得到回答时，请点击 "Mark as answer" 来标记答案。

许多当前的讨论问题是从旧的 Issues 中迁移过来的，很多都有 `answered` 标签，表示在 Issues 中已经得到了回答，但现在在 GitHub Discussions 中，还不清楚哪些是实际的回答。

你可以通过 <a href="https://github.com/fastapi/sqlmodel/discussions/categories/questions?discussions_q=category:Questions+is:open+is:unanswered" class="external-link" target="_blank">过滤未回答的问题</a>。