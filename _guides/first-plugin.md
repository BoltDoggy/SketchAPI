---
title: 你的第一个插件
summary: Basic concepts for plugin users.
permalink: /guides/first-plugin/
order: 110
---

本文档将带着你创建第一个 Sketch 插件 -- Hello World，并解释基本的 Sketch 可扩展性概念。

在此次练习中，你将为 Sketch 添加一个新的命令，该命令会显示一个简单的 "Hello World"。在稍后的练习中，你将会和 Sketch 画布进行交互并查询用户当前所选中的图层。

## 准备环节

你需要在 `$PATH` 中安装可用的 [Node.js](https://nodejs.org/en/)，包括 [npm](https://www.npmjs.com/) -- Node.js 包管理器，它将用于为 Sketch 插件开发者安装工具链。

## 生成一个新插件

为 Sketch 添加自己功能的最简单的方式就是添加命令。一个命令注册一个回调函数，该回调函数可以通过插件菜单或者键绑定来调用。

我们写了一个名为 [skpm](https://github.com/skpm/skpm) 的小型工具链来帮助你入门。安装 `skpm` 并搭建脚手架，然后来创建我们的新插件吧：

```bash
npm install -g skpm

skpm create my-plugin

cd my-plugin
```

## 运行你的插件

- 构建插件：`npm run build`
- 启动 Sketch 并打开一个文件
- 选择 `Plugins` > `my-plugin` > `My Command`
- 恭喜你！你刚刚创建并执行了你的第一个 Sketch 命令

## 插件的结构

运行后，生成的插件应具有如下的结构：

```
.
├── .gitignore
├── README.md
├── src                         // sources
│   ├── manifest.json           // plugin's manifest
│   └── my-command.js           // source code of the command
├── node_modules
│   └── skpm                    // the sketch plugin developer toolchain
├── my-plugin.sketchplugin      // compilation output, the actual plugin
│   └── Contents
│       ├── Resources
│       └── Sketch
│           ├── manifest.json
│           └── my-command.js
└── package.json
```

让我们来看看所有这些文件存在的目的并解释一下它们的作用：

### 插件清单：`manifest.json`

- 每个 Sketch 插件都必须有一个 manifest.json 文件以描述它及它的功能。
- Sketch 在启动期间会读取该文件。
- 有关更多信息请查阅 `manifest.json` [manifest 引用](https://developer.sketchapp.com/guides/plugin-bundles/#manifest)。

### `package.json`

如果你之前查看过 nodejs 包，那你必然对 `package.json` 非常熟悉。它描述了你的包（在本例中是你的插件）的依赖关系，并且包含了一些关于它的元数据。

你会注意到里面有一段特殊区域：`skpm`。你可以在这里指定有关你插件的元数据（而不是在 `manifest.json` 文件中）。根据经验，我通常会将命令信息放在 `manifest.json` 中，而将其它所有的信息放在 `package.json` 中（sknpm 在编译时会将这些信息添加进 manifest.json 中，这样你就不必去手动复制它们了）。

### `src/my-command.js`

这个文件用来定义插件命令。它由 `manifest.json` 引用并且必须导出一个函数。

## 一个简单的改变

在 `src/my-command.js` 中，尝试着替换命令实现以展示所选中的图层数。

```js
export default function() {
  const doc = sketch.getSelectedDocument()
  const selectedLayers = doc.selectedLayers
  const selectedCount = selectedLayers.length

  if (selectedCount === 0) {
    sketch.UI.message('No layers are selected.')
  } else {
    sketch.UI.message(`${selectedCount} layers selected.`)
  }
}
```

运行 `npm run build` 以重建插件。打开一个 Sketch 文件，并选中一些图层。当你运行 `my-plugin` 命令时你就可以看到所选中的图层数量了。

> 专业提示：你可以通过运行 `npm run watch` 命令来自动重建插件。

## 发布你的扩展

阅读有关如何 [分享插件](https://developer.sketchapp.com/guides/publishing-plugins/) 的信息。

## 下面的步骤

在这次练习中，我们探讨了一个非常简单的插件。

如果你想要更全面地阅读插件API，那么可以从以下几个主题着手：

- [扩展API概述](https://developer.sketchapp.com/reference/) - 了解 Sketch 可扩展性的可能性。
- [其它插件示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples) - 看看我们的示例插件项目列表。
