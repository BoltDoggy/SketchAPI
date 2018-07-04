---
title: 插件基础
summary: Basic concepts for plugin users.
permalink: /guides/
redirect_from: /introduction/
order: 100
---

在磁盘上，插件只是以特定布局排列的文件夹。

它包含一个或多个脚本。每个脚本定义一个或多个以某种方式扩展 Sketch 的命令。它还可以包含命令用来执行任何操作的任何其他可选资源（例如图像）。

插件脚本使用 JavaScript 编写。

## 术语

在我们进一步讨论之前，让我们定义一些术语。

* _Plugin(插件)_: 一组 _Scripts、Commands_ 和其他资源组合在一起作为一个独立单元
* _Plugin Bundle(插件包)_: 磁盘上的文件夹，其中包含组成 _Plugin_ 的文件
* _Action(行为)_: 用户所做的事情（选择菜单或更改文档）触发 _Command_
* _Command(命令)_: 一个插件可以定义多个命令; 通常每一个都与不同的菜单或键盘快捷键相关联，并导致执行不同的 _Handler_ 程序。
* _Handler(操作)_: 执行一些代码来实现 _Command_ 的函数。
* _Script(脚本)_: 包含一个或多个实现一个或多个 _Commands 的处理程序*的 JavaScript 文件。

## 我如何制作插件？

到现在为止，您可能想知道如何开始自己编写。

开始使用插件最简单的方法是打开 Sketch，打开文档并 `control + shift + k` 打开 `Run Script` 面板。你不需要安装任何东西; 你可以打开它并在那里实验。如果您想使用真实的开发环境（您需要为了分发您的插件），请查看[开发环境](/guides/preferences)页面。

最小的插件示例如下所示:

```js
export default function(context) {
  context.document.showMessage('Hello, world!')
}
```

它在 Sketch 文档底部弹出一个提示 “Hello，world！”。

接下来的几个指南将逐渐向您介绍插件的内部工作。我们将检查插件的构建块：[清单](/guides/plugin-bundles/)和脚本。一旦你掌握了它们，你可以创建复杂的插件！

## 关于JavaScript的说明

Sketch 插件是用 JavaScript 编写的，因此我们假设您对 JavaScript 语言有基本的了解。如果您觉得不太自信，我们建议 [您复习 JavaScript 知识](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)，以便更轻松地进行跟进。

我们还在示例中使用了一些 ES6 语法。我们尽量少用，因为它还是比较新的，但我们鼓励您熟悉[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，[let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) 和 [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) 语句。

该脚本不在浏览器或 node 环境中运行，而是在 native MacOS 和 Sketch API 都暴露的[特殊环境](/guides/cocoascript/)中运行。它有点高深，但必须真正了解如何构建更高级的东西。
