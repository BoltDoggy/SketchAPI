---
layout: single-page
title: 参考
permalink: /reference/
script: /js/search.js
---

Sketch 中的插件系统使您可以完全访问应用程序的内部结构和 macOS 中的核心框架。 所以你有巨大的力量来建立几乎 _anything_。

但是，强大的功能带来了巨大的责任，因此您需要在每个 Sketch 版本中密切关注您的代码。 我们在重构时不时更改 Sketch 的内部结构，因此您的插件可能会调用一些重命名或删除的方法。

我们确实意识到这当然不是理想的。 这就是我们支持内部和插件之间的 JavaScript API 的原因。 我们希望它涵盖 90％ 的用例。 如果没有，您可以随时使用自己的风险进入内部。

下面的页面包含插件可以侦听的所有操作的简要说明，以及它们可以与之交互的一些关键 Sketch 类。 这是 JavaScript API，它在 Sketch 版本中保持稳定。

* [Javascript API](/reference/api)
* [Actions](/reference/action)

即使我们不打算记录内部信息，也可以查看 3 种信息来源：

* [The official AppKit document](https://developer.apple.com/documentation/appkit?language=objc): 这是 Apple 框架 Sketch 的基础.
* [Foundation](https://developer.apple.com/documentation/foundation?language=objc): 更重要的 Apple 课程和服务.
* [The Sketch Headers](https://github.com/abynim/Sketch-Headers) (Thanks @abynim 🙏): 这是 Sketch 使用的所有类的标题。 如果您的插件因为使用了已删除的方法而中断了新版本，则可以检查差异以查找替换。

同样，最后一个链接是使用风险，我们不会记录或冻结任何此类链接，但我们希望赋予您执行任何操作的权力。

要了解如何使用这些 Objective-C 类，请查看 [CocoaScript document](/guides/cocoascript/).
