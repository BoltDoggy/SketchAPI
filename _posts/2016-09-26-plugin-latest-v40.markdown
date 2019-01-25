---
title: Sketch 40中的插件有什么新功能？
categories: scripting update
---

## 脚本 API

随着Sketch版本40的发布，我们正在逐步推出我们的 Javascript API，作为在插件中使用 Sketch 模型的更好方法。

作为此项工作的一部分，我们已更新此网站以包含 [API参考](/reference/api)部分。

API本身仍处于开发阶段，不应被视为完全固定或准备好黄金时段。

但它已经到了那里，如果你开始研究新的插件，你可能要考虑尝试一下。

在接下来的几个版本中，我们将进一步开发API，更新所有现有的 [插件示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples/) 以使用API，以及 添加一些新的例子。

## NSAppTransportSecurity

使用 Sketch 的下一个版本，我们计划完全采用 Apple 之前称为 App Transport Security 的更改。 这与 HTTP 连接的安全性有关，并要求所有这些请求都使用 `https：` 而不仅仅是普通的 `http：`。

到目前为止，我们已禁用此功能，但很快我们就会启用它。

除非您的插件从网页请求内容，否则它不应受此更改的影响。

但是，如果您从网页中获取数据或资源，则需要在URL中使用 “https：” 切换。

我们还建议每个人都使用 `https：` 来更改插件清单文件中指定的任何URL。 Sketch的未来版本可能会使用这些URL。