---
title: 插件新闻
categories: scripting update
---

在过去的几周里，我们一直在努力工作，对 Plugin 系统进行了一些更新。

其中一些将与即将发布的 3.8 版本 [现在处于测试阶段](http://www.sketchapp.com/beta/) 一起推出。 其他人将在稍后出现，但在这两种情况下，我们都希望给开发者社区中的每个人一些预警。

## 弃用的 API

正如你们许多人所知，我们经常需要在内部更改代码，这有时意味着我们已经公开的API已被弃用。 到目前为止，我们还倾向于将旧的API留在代码中，并让他们吐出一个控制台消息，说它们已被弃用。

向前迈进，我们打算开始实际删除这些API。 我们会给你一个或两个版本的通知（所以如果我们弃用 3.7 中的东西，我们不会删除它直到 3.9），但是你应该知道弃用将不再意味着 “你可以忽略它”，现在意味着 “你真的应该停止使用这个”。

## 传统插件

在类似的说明中，回到3.3版本，我们为插件引入了一种新的捆绑格式。 它有很多好处，但为了简化过渡，我们继续支持旧的单文件插件。

为了简化我们的代码并允许我们添加新的脚本功能，我们打算弃用然后删除对旧插件的支持。 从版本3.9开始，旧式插件提供的任何命令都将分组到菜单中的“Legacy”标题下。

接下来，更高版本的Sketch将不再加载旧插件。

我们建议您立即将插件移至新格式！ 这是一项非常简单的工作，它将为您提供面向未来的证明。

我们确信你现在已经完全卖掉了这个变化，但还有一点点推荐，以防万一：如果你想在我们的网站上推出你的插件，你需要将它们改成新的格式！

## Action API
使用3.8，我们为插件引入了很多请求的能力，以便能够响应用户在Sketch中执行的操作。

We have posted [some documentation](/reference/action/) and [example Plugins](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples/) for action support.

我们现在要清楚地表明这是行动支持的1.0版本，接下来会有更多内容。 我们意识到它现在的工作方式存在一些不一致之处，并不是用户可以做的所有事情都可以开始使用。 值得一提的是，出于性能原因，某些事情可能永远不可用作动作。

Even having said that though, this feature should greatly expand the range of things that Plugins can usefully do, and we look forward to seeing what you do with it. Please [send us feedback](mailto:mail@sketchapp.com) on how it works for you, and what you’d like to see change.

## Scripting API

正如你们中的一些人可能已经注意到的那样，我们在 3.7 中尝试添加一个只能使用 JavaScript 的 API，这是插件可以调用的。

这样做的目的是直接从 JavaScript 创建一个更小，更稳定的插件功能集。 每次我们发布 Sketch 的新版本时，我们都会尝试确保 API 继续有效。 这应该使用 API 来限制使用 API 的当前情况，他们被迫使用内部 Sketch 代码，然后在我们更改代码时破坏。

版本 3.8 确实包含稍微更新的版本，但我们认为它不适合黄金时段，并且没有正确记录。 API 的设计在这一点上也肯定不稳定，我们不建议使用它，因为类和方法的名称可能会改变。

考虑到这是一个早期的警告，它即将到来，请再次 [发送反馈](mailto:mail@sketchapp.com) 了解您希望如何运作。
