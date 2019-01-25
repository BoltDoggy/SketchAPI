---
title: Action(行为) API
summary: API to let plugins react to events in the app.
permalink: /guides/action-api/
order: 140
---

在 Sketch 3.8 中，我们介绍了 Action API：一种让插件对应用程序中的事件做出反应的方法。使用它，插件作者可以编写在触发某些操作时执行的代码，例如“打开文档”，“保存”，“添加画板”......

## 什么是 action?

action 是应用程序中发生的事件，通常是用户交互的结果。 action 的名称如 `CloseDocument`，`DistributeHorizontally` 或 `TogglePresentationMode`，你可以告诉你的插件在触发这些动作时运行一些代码。

## 如何注册我的插件以 "监听" 某个 action?

简单：您只需在插件已有的 `manifest.json` 文件中添加一个处理程序。

我们将为 `OpenDocument` 操作添加一个新的处理程序：

```diff
"commands" : [
  ...
+  {
+    "script" : "my-action-listener.js",
+    "name" : "My Action Listener",
+    "handlers" : {
+      "actions": {
+        "OpenDocument": "onOpenDocument"
+      }
+    },
+    "identifier" : "my-action-listener-identifier"
+  }
  ...
],
```

我们告诉我们的插件我们想在文档打开时运行 `onOpenDocument` 函数，所以让我们在 `my-action-listener.js` 中添加它：

```js
export function onOpenDocument(context) {
  context.actionContext.document.showMessage('Document Opened')
}
```

保存所有内容，构建插件，现在无论何时在 Sketch 中打开文档，您都应该看到一个小的 Toast 横幅，上面写着 “Document Opened”。

## Action 上下文

触发操作时，Sketch 可以向目标函数发送有关操作本身的一些信息（如选择更改时的选定图层，或打开新文档时的当前文档）。 我们称之为 Action Context，您可以使用 `context.actionContext` 从 `context` 访问它，该 `context` 是作为目标函数的参数发送的。

但请记住，**并非所有操作都设置了动作上下文**。 事实上，他们中的大部分目前都没有，所以如果你认为你想在行动背景中访问某些内容，请给我们发一条说明，我们会尽快添加。

## `begin`/`finish` actions

一些动作（如 `SelectionChanged` ）实际上分两个阶段发生：`begin` 和 `finish`。 如果只想在其中一个函数上调用函数，可以为 `SelectionChanged.begin` 或 `SelectionChanged.finish` 添加一个处理程序。 如果您不添加任何内容，操作将被触发两次。

## 找到正确的 action

有关 API 中所有可用操作的列表，请查看 [操作参考部分](/reference/action)。

> 专业提示：有时浏览列表是太多操作，你只是想要更直接的东西。 对于这些情况，您可以 [监听所有操作](/guides/preferences/#listen-to-all-actions-in-the-action-api) 找到您需要的那个。

同样，如果您希望看到添加到列表中的任何事件，请告诉我们，我们将尝试添加它（由于性能原因，某些事件不在列表中，例如 “图层被拖动”）。

## 下一步

如果您想更全面地阅读 Action API，请尝试以下主题：

* [Action API 参考](/reference/action/) - 了解可用操作的完整列表。
* [其他插件示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples) - 查看我们的示例插件项目列表。
