# FastAPI 和 Pydantic - 简介

**SQLModel** 最突出的应用场景之一，也是它创建的主要原因，就是与 **FastAPI** 结合使用。✨

<a href="https://fastapi.tiangolo.com/" class="external-link" target="_blank">FastAPI</a> 是一个用于构建 Web API 的 Python Web 框架，由 **SQLModel** 的作者创建。FastAPI 也构建在 **Pydantic** 之上。

在这一组章节中，我们将看到如何将 SQLModel **表模型**（表示 SQL 数据库中的表，和我们到目前为止看到的所有模型一样）与 **数据模型**（仅表示数据，实际上是在幕后使用 Pydantic 模型）结合使用。

能够将 SQLModel **表** 模型与纯 **数据** 模型结合使用本身就很有用，但为了让所有示例更加具体，我们将与 **FastAPI** 一起使用它们。

到最后，我们将拥有一个 **简单** 但 **完整** 的 Web **API**，用于与数据库中的数据进行交互。🎉

## 学习 FastAPI

如果你从未使用过 FastAPI，可能在继续之前先去了解一下它会是个不错的选择。

只需阅读并尝试一下 <a href="https://fastapi.tiangolo.com/" class="external-link" target="_blank">FastAPI 官方页面</a> 上的示例就足够了，应该不会花费你超过 **10 分钟**。