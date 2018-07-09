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

## 始终在运行前重新加载脚本

出于性能原因，Sketch 会缓存 Plugins 文件夹的内容。这对用户来说非常方便，因为插件运行速度非常快，但如果你是开发人员，会让你的生活变得艰难。这就是我们添加首选项以禁用此缓存机制并强制 Sketch 始终从磁盘重新加载插件代码的原因：

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist AlwaysReloadScript -bool YES
```

如果您启用此功能，只要您保存脚本，它就可以在 Sketch 中进行测试（对重启说再见, 如果只是为了测试一个小小的更改！）

请注意，此设置用来确定, 在每当 Sketch 为脚本创建新的 javascript 上下文时, 是否从磁盘盘重新加载脚本源。如果是 `NO`，则缓存源，如果是 `YES`，则始终从光盘重新加载源。

然而，它没有做的是在创建新的 JavaScript 上下文时进行更改。对于长时间运行的脚本，相同的上下文保存在内存中（必须是 - 正在运行的脚本正在使用它），直到脚本退出。因此，如果您正在测试一个长时间运行的脚本，您仍然必须找到一种方法来停止脚本，以便抛弃上下文（这通常意味着重新启动 Sketch 或设置 `coscript.setShouldKeepAround(false)`）。

### 更改插件 JavaScript 后始终重新启动 sketch

如果您发现自己属于后一类，需要在开发期间定期重新启动长时间运行的 JavaScript 上下文，那么 unix 实用程序 [`entr`](http://entrproject.org/) 可能会派上用场。给定 stdin 上的文件列表，`-r` 每次修改其中一个文件时，它将运行一个命令（或重新启动一个长时间运行的进程）：

`find /your/plugin/build/dest -name '*js' | entr -r /Applications/Sketch.app/Contents/MacOS/Sketch`

#### 与 webview JavaScript 结合使用

如果您碰巧也有不需要重新启动 Sketch 的 webview JavaScript（因为右键单击 + 重新加载已经足够了），请确保避免将这些文件传递给 `entr`：

`find ... | grep -v 'web\.js' | entr ...`

## 检查 WebView

如果您的插件使用的是 webview，那么您可能需要在某个时候进行检查。

为此，您需要添加首选项：

```shell
defaults write com.bohemiancoding.sketch3 WebKitDeveloperExtras -bool true
```

然后，您只需右键单击您的 webview 并单击 `Inspect` 即可。检查器就将会出现。
