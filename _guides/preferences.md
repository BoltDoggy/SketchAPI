---
title: 开发环境
summary: Some preferences to make developing Plugins a bit more pleasant
permalink: /guides/preferences/
redirect_from: /introduction/preferences/
order: 120
---

如果您开发 Plugins for Sketch 花费了不少重要时间，则可以使用这些首选项配置对工作流程进行一些改进。

由于并非所有 Sketch 用户都是插件开发人员，因此在 “Preferences” 面板中为这些首选项设置 UI 并没有任何意义。您需要使用 Terminal.app 来启用/禁用它们。

## 为插件定义代码编辑器

有一个喜欢的代码编辑器？你可以告诉 Sketch 使用它来编辑插件。例如，如果你使用 [Atom](https://atom.io/)，你可以这样做：

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist "Plugin Editor" "/usr/local/bin/atom"
```

并重新启动 Sketch，您会看到一些新的菜单项：

* 转到 “Preferences” > “Plugins”，然后右键单击任何列出的插件。您将看到一个 “Edit Code…” 选项，该选项将启动编辑器并打开所选的插件代码。
* 打开插件菜单，你会看到一个 'Edit Plugins…' 选项，它将启动你的编辑器并打开整个 'Plugins' 文件夹。

## 调整 “Custom Plugin…” 编辑器

要更改 “Run Script…” 面板中使用的字体（例如，使用 SF Mono），可以这样做：

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFont "SF Mono Light"
```

要回到默认设置（Andale Mono），只需删除首选项：

```shell
defaults delete ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFont
```

要更改编辑器的字体大小（默认值为12），请使用

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFontSize 14
```

## 监听 Action API 中的所有操作

<p class="warning">
  <strong>警告：</strong> 这是一项非常昂贵的操作，并且会影响 Sketch 的性能。请 <em>仅在您的开发系统上</em> 使用此功能, 而 <strong>不要在客户的计算机上启用此功能</strong>.
</p>

当使用新的 [Action API](/reference/action/) 工作时，你可能想监听多个事件（试图找到监听的多个事件中*哪个*是您要使用的一个）。

为此，请使用 `actionWildcardsAllowed` 首选项。如果设置为`YES`，则允许脚本为事件注册通配符处理程序。这是默认关闭的，它可能会对性能产生不利影响，因此请小心处理。

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist actionWildcardsAllowed -bool YES
```

一旦你这样做了，你可以通过在你的 `manifest.json` 中的 `handlers.actions` 对象中添加一个 `*` 键来告诉你的插件为每个动作调用一个方法：


```diff
{
  ...
  "handlers": {
+    "actions": {
+      "*": "onActionHandler"
+    }
  }
  ...
}
```

## Always reload scripts before running

For performance reasons, Sketch caches the contents of the Plugins folder. This is very convenient for users, since Plugins run very fast, but makes your life hard if you’re a developer. That’s why we added a preference to disable this caching mechanism and force Sketch to always reload a Plugin’s code from disk:

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist AlwaysReloadScript -bool YES
```

If you enable this, as soon as you save your script it will be ready for testing in Sketch (bye bye relaunching it just to test a small change!)

Please note that this setting determines whether the source of the script is reloaded from disc whenever Sketch makes a new javascript context for the script. If it’s `NO`, the source is cached, if it’s `YES`, the source is always reloaded from disc.

What it doesn’t do, however, is change when a new JavaScript context is made. For long-running scripts, the same context is held in memory (it has to be — the running script is using it) until the script exits. So if you’re testing a long-running script, you will still have to find a way to stop the script, so that the context gets thrown away (that usually means relaunching Sketch, or setting `coscript.setShouldKeepAround(false)`).

### Always restarting sketch after changes to plugin JavaScript

If you find yourself in the latter category, of needing to restart long-running JavaScript contexts regularly during development, the unix utility [`entr`](http://entrproject.org/) may come in handy. Given a list of files on stdin, it will run a command (or restart a long-running process given `-r`) every time one of those files is modified:

`find /your/plugin/build/dest -name '*js' | entr -r /Applications/Sketch.app/Contents/MacOS/Sketch`

#### In conjunction with webview JavaScript

And if you so happen to also have webview JavaScript that doesn't require rebooting Sketch (because right-click + reload is fine), just make sure to avoid passing those files to `entr`:

`find ... | grep -v 'web\.js' | entr ...`


## Inspect a WebView

If your plugin is using a webview, chances are that you will need to inspect it at some point.

To do so, you need to add the preference:

```shell
defaults write com.bohemiancoding.sketch3 WebKitDeveloperExtras -bool true
```

Then you can simply right-click on your webview and click on `Inspect`. The inspector should show up.
