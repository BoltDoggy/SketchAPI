---
title: Action(行为) API
summary: API to let plugins react to events in the app.
permalink: /guides/action-api/
order: 140
---

在 Sketch 3.8 中，我们介绍了 Action API：一种让插件对应用程序中的事件做出反应的方法。使用它，插件作者可以编写在触发某些操作时执行的代码，例如“打开文档”，“保存”，“添加画板”......

## 什么是 action?

An action is an event that happens in the app, usually as a consequence of a user interaction. Actions have names like `CloseDocument`, `DistributeHorizontally` or `TogglePresentationMode`, and you can tell your plugin to run bits of code when those actions are triggered.

## 如何注册我的插件以 "监听" 某个 action?

Easy: you just add a handler in the `manifest.json` file that your plugin already has.

We're going to add a new handler, for the `OpenDocument` action:

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

We're telling our plugin that we want to run the `onOpenDocument` function when a document is open, so let's add that in `my-action-listener.js`:

```js
export function onOpenDocument(context) {
  context.actionContext.document.showMessage('Document Opened')
}
```

Save everything, build the plugin and now whenever you open a document in Sketch you should see a small toast banner saying "Document Opened".

## Action 上下文

When an action is triggered, Sketch can send the target function some information about the action itself (like the selected layers when the selection changes, or the current document when a new document is open). We call that the Action Context, and you can access it from the `context` that is sent as a parameter of the target function using `context.actionContext`.

Keep in mind, though, that **not all actions set an Action Context yet**. In fact, most of them don't at the moment, so if you think there's something you'd like to access in an Action Context, send us a note and we'll add it as soon as possible.

## `begin`/`finish` actions

Some actions (like `SelectionChanged`) actually happen in two phases: `begin` and `finish`. If you want to call a function only on one of them, you can add a handler for `SelectionChanged.begin`, or `SelectionChanged.finish`. If you don't add anything, the action will be triggered twice.

## 找到正确的 action

For a list of all the available actions in the API, check the [Actions Reference section](/reference/action).

> Pro Tip: sometimes browsing the list is too much work, and you just want something a bit more direct. For those cases, you can [listen to all the actions](/guides/preferences/#listen-to-all-actions-in-the-action-api) to find the one you need.

Again, if there's any event you'd like to see added to the list, let us know and we'll try to add it (some events are not on the list for performance reasons, like "layer is dragged").

## 下一步

如果您想更全面地阅读 Action API，请尝试以下主题：

* [Action API 参考](/reference/action/) - 了解可用操作的完整列表。
* [其他插件示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples) - 查看我们的示例插件项目列表。
