---
title: Debugging
summary: How to debug a sketch plugin
permalink: /guides/debugging-plugins/
order: 130
---

在开发 Sketch 插件时，你可能需要一些方法来了解代码运行时发生了什么。

## 日志

调试 JavaScript 代码最通常的做法就是在关键步骤处放入一堆 `console.log`。但不幸的是，JavaScriptCore （[运行 Sketch 插件的环境](https://developer.sketchapp.com/guides/cocoascript/)）没有提供 `console` 方法。作为替代，可以使用一个名为 `log` 的特殊方法。

有以下几种选择可以查看日志：

* 打开 Console.app 并查找 sketch 日志

* 查看 `~/Library/Logs/com.bohemiancoding.sketch3/Plugin Output.log` 文件

* 运行 `skpm log` 命令，该命令可以输出上面的文件（执行 `skpm log -f` 可以流式地输出日志）

`skpm` 也是 `console` 的 一个polyfill，这样你就可以像往常一样使用 `console.log` 了。除了在后台调用 `log` 方法以外，它还会将日志转发到 [sketch-dev-tools](https://github.com/skpm/sketch-dev-tools)。

## `调试器` 和变量检查

当插件运行时，Sketch 将会创建一个与其关联的 JavaScript 上下文。我们就可以使用 Safari 来调试该上下文。

在 Safari 中, 打开 `开发` > _`你的机器名称`_ > `自动显示 JSContext 的网页检查器`. 并且你可能希望启用 `自动暂停连接到 JSContext`，否则检查器将在你可以与它交互之前关闭（当脚本运行完时上下文会被销毁）。

现在你就可以在代码中使用断点了，也可以在运行时检查变量的值等等。

## Objective-C 类的内省

Sketch 中的插件系统使你可以完全访问应用程序的内部结构和 macOS 中的核心框架。Sketch 是用 Objective-C 构建的，它的类桥接到了 JavaScript。了解你正在处理的类以及在这些类上定义的方法通常会很有用。

你可以使用桥接器定义的一些内省方法来访问这些信息。例如：

```js
String(context.document.class()) // MSDocument

var mocha = context.document.class().mocha()

mocha.properties() // array of MSDocument specific properties defined on a MSDocument instance
mocha.propertiesWithAncestors() // array of all the properties defined on a MSDocument instance

mocha.instanceMethods() // array of methods defined on a MSDocument instance
mocha.instanceMethodsWithAncestors()

mocha.classMethods() // array of methods defined on the MSDocument class
mocha.classMethodsWithAncestors()

mocha.protocols() // array of protocols the MSDocument class inherits from
mocha.protocolsWithAncestors()
```

## Sketch-dev-tools

我们创建了一个小型的特殊 Sketch 工具来帮助你调试你的插件，并且希望你能够因此而轻松一些。它采用了 Sketch 插件的形式，你可以在[这里](https://github.com/skpm/sketch-dev-tools/releases/tag/v0.6.0)下载并使用 `cmd + option + j` 来启动。
