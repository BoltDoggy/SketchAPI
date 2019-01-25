---
title: Plugin Bundles
permalink: /guides/plugin-bundles/
redirect_from: /introduction/plugin-bundles/
order: 200
---

Plugin(插件) 是一个或多个 **scripts(脚本)** 的集合。 每个 script 定义一个或多个 **commands(命令)** ，它们以某种方式扩展 Sketch。

在磁盘上，Plugin 是一个带有 `.sketchplugin` 文件扩展名的文件夹，包含文件和子文件夹。

严格来说，Plugin 实际上是 [OS X package(包)](https://developer.apple.com/library/mac/documentation/CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1)，用作为 [OS X bundle(束)](https://developer.apple.com/library/mac/documentation/CoreFoundation/Conceptual/CFBundles/AboutBundles/AboutBundles.html#//apple_ref/doc/uid/10000123i-CH100-SW1)。

package 是 Finder 向用户呈现的任何目录，就好像它是单个文件一样（您可以使用 Finder 中的 `Reveal Package Contents(显示包装内容)` 命令查看内部）。

bundle 是具有标准化分层结构的目录，其保存可执行代码和该代码使用的资源。

Sketch 插件不允许原生编译代码，但我们使用标准的 bundle 布局（例如，资源，位于 bundle 中的 Resources/ 文件夹中），特定于插件的文件位于 Sketch/ 目录中。

## Plugin Bundle 文件夹结构

Bundles 包含一个 `manifest.json` 文件，一个或多个 `.cocoascript` 文件（包含用 CocoaScript 或 JavaScript 编写的脚本），它们实现了 Plugins 菜单中显示的命令，以及任意数量的共享库脚本和资源文件。

这是一个例子：

```
mrwalker.sketchplugin
  Contents/
    Sketch/
      manifest.json
      shared.js
      Select Circles.cocoascript
      Select Rectangles.cocoascript
    Resources/
      Screenshot.png
      Icon.png
```

最关键的文件是 `manifest.json` 文件，它告诉 Sketch 其他所有东西。

## Manifest(清单)

manifest 是一个 JSON 文件，其中包含有关插件，其命令和资源的元数据。

它描述了诸如全名，描述和作者姓名之类的内容。 它列出了插件定义的任何命令的名称，并告诉 Sketch 如何调用相应的菜单项以及将它们放入哪些菜单。

这是一个例子：

<a id="manifest-json-example"></a>

```json
{
  "name": "Select Shapes",
  "description": "Plugins to select and deselect shapes",
  "author": "Joe Bloggs",
  "homepage": "https://github.com/example/sketchplugins",
  "version": "1.0",
  "identifier": "com.example.sketch.shape-plugins",
  "appcast": "https://excellent.sketchplugin.com/excellent-plugin-appcast.xml",
  "compatibleVersion": "3",
  "bundleVersion": 1,
  "commands": [
    {
      "name": "All",
      "identifier": "all",
      "shortcut": "ctrl shift a",
      "script": "shared.js",
      "handler": "selectAll"
    },
    {
      "name": "Circles",
      "identifier": "circles",
      "script": "Select Circles.cocoascript"
    },
    {
      "name": "Rectangles",
      "identifier": "rectangles",
      "script": "Select Rectangles.cocoascript"
    }
  ],
  "menu": {
    "items": ["all", "circles", "rectangles"]
  }
}
```

This Plugin is called “Select Shapes”. It defines three commands “All”, “Circles”, and “Rectangles”, which will be placed in a “Select Shapes” menu.

This plugin can be updated by Sketch. Sketch will download the file at the location specified by `appcast` and use it to determine if an update is available or not.

Unpacking this file a bit further, here are the supported keys and what they are for:

#### `name`

The name for this Plugin. This is used by default as the name of the sub-menu in which the Plugin’s menu commands will appear.

#### `description`

A string describing what this Plugin’s command (or commands) do.

#### `author`

A string specifying the author of the Plugin.

#### `authorEmail`

An optional string specifying how to contact the Plugin author by email.

#### `icon`

The resource path to the icon for this Plugin. This is used in the Plugin list and when showing an alert from your plugin. Must be a 128x128 PNG.

#### `homepage`

An optional string specifying an online resource for the user to find out more information or provide feedback for the Plugin.

#### `version`

A string specifying the [semantic version][semantic versioning] for the Plugin, for example `1.0`, `1.1.1`.

#### `identifier`

A string specifying a unique identifier for the Plugin.

Reverse-domain syntax is strongly encouraged, for example `com.example.sketch.shape-plugins`.

Sketch uses this string internally to track the Plugin, store settings for it, etc.

#### `appcast`

A string specifying a URL to the appcast file. The appcast file contains information about updates to the plugin, like the versions of available updates and where the updates can be downloaded from. Sketch downloads this file to determine if there are plugin updates available.

#### `compatibleVersion` and `maxCompatibleVersion`

A string specifying the [version][semantic versioning] of Sketch in which the author has tested the Plugin, for example `3`, `3.1`, `3.2.2`.

At the moment (Sketch 3.4) this is an optional key, but we could use it as a filtering option in the [Plugins page](https://www.sketchapp.com/plugins/) at some point.

Internally it uses the [BCCompareVersions](https://github.com/BohemianCoding/BCFoundation/blob/develop/Source/BCVersionComparison.m#L11) function, which splits the string by `.`, then compares the integer value of each component.

#### `bundleVersion`

The version for the layout of the bundle an metadata. If excluded it is assumed the value is 1.

This is just a future-proofing mechanism for us. If, in the future, we see a plugin with a bundleVersion > 1, we'd know that we could treat the other values in the metadata differently.

For now it's fine to omit it.

#### `suppliesData`

Indicates if the plugin supplies some data. If `true`, it results in the data icon showing in the plugin preferences for the plugin.

#### `disableCocoaScriptPreprocessor`

This is an advanced setting, and it defaults to `false`. When set to `true`, it will disable CocoaScript's own preprocessor. This way, you'll be able to use build systems like [skpm](https://skpm.io) or ES6 module syntax to develop your plugins.

Setting this option to `true` does the following:

- disables `@import` support, you'll have to take care of your imports manually
- disables bracket syntax (i.e: `[obj msg:]`), you'll have to use dot-syntax only

#### `commands`

An array of commands that the Plugin defines.

Each item within the array is a dictionary specifying the name, shortcut and other properties of the command. See [Plugin Commands](#plugin-commands) for more details.

#### `menu`

A dictionary describing the menu layout for the commands in this Plugin.

See [Plugins Menu](#plugins-menu) for more details on the contents of this dictionary, and how the menu for each Plugin is built.

#### `scope`

If present, and set to `"application"`, enables the plugin to be run when there are no documents open in Sketch.

## Plugin Commands

A Plugin defines one or more commands for the user to execute.

The commands array in the manifest describes these. Each entry in the array is a dictionary, with the following properties:

#### `name`

The display name for the command. This value is used in the Plugins menu.

#### `identifier`

A string specifying a unique identifier for the command within the Plugin bundle. This is used to consistently map commands to actions, irrespective of command name changes.

#### `shortcut`

An optional string specifying a default keyboard shortcut for this command, for example: `ctrl t`, `cmd t`, `ctrl shift t`.

#### `script`

The relative path within the Plugin bundle’s `Sketch` folder for the script that implements this command.

#### `handler`

The name of the function with the script to call this command. The function must take a single `context` parameter, which is a dictionary with keys for things like the current document and selection. If unspecified the command is expected to be `onRun`:

```js
var onRun = function (context) {
  var doc = context.document;
  var selection = context.selection;
  …
}
```

## Plugins Menu

When it loads a Plugin, Sketch creates a menu for it, and populates that menu using information from the “menu” dictionary in the manifest file.

This dictionary can contain the following keys.

#### `title`

A string specifying the title to use for the submenu.

#### `items`

This is an array which lists the items to include in the menu.

It can contain items of two types:

- a string giving the identifier of a command
- a dictionary describing a sub-menu (containing "title" and "items")

#### `isRoot`

By default, the menu items listed in this dictionary will appear in a menu with the name specified by the _title_ key.

If the isRoot key is specified, with a value of true, the items will instead be inserted at the root level of the Plugins menu, rather than in a subfolder. In this case, the _title_ key will be ignored.

_This key is ignored in sub-menus._

### Menu Example

Here’s an example. It defines three commands in a menu called “My Plugin Menu”. The first two items of the menu correspond to two of the Plugin’s commands, but the third item is a submenu called “My Plugin Submenu”. This submenu has a single item in it (the third of the Plugin’s commands):

```json
{
  "menu": {
    "title": "My Plugin Menu",
    "items": [
      "command1-identifier",
      "command2-identifier",
      {
        "title": "My Plugin Submenu",
        "items": ["command3-identifier"]
      }
    ]
  }
}
```

## Handlers

Plugin commands are implemented by handlers.

These are simply JavaScript functions which live in a `.cocoascript` file in the Plugin bundle, and which take a single parameter containing some context.

Here’s a simple example:

```js
var doMyCommand = function(context) {
  context.document.currentPage().deselectAllLayers()
}
```

In the manifest file, you specify a dictionary describing each command that the Plugin defines.

In this dictionary, _script_ and _handler_ keys tell Sketch which script file to look in, and which handler to run.

You are free to put each command implementation into its own script file, or to put them all in a single file.

You must specify the _script_ key for each command.

If you put each command in its own script file, you can omit the _handler_ key. In this case, Sketch will default to calling the `onRun` handler.

If you put multiple command handlers into the same script file, you need to use the _handler_ key for each one, since they can’t all use the `onRun` handler!.

[semantic versioning]: http://semver.org
