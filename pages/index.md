---
title: 扩展 Sketch
permalink: /
---

我们努力使 Sketch 成为梦想中的“设计师工具箱”。但每个人的需求略有不同，也许你需要一个我们没有实现的功能。不用担心：[插件很有可能已经满足您的需求](https://sketchapp.com/extensions/plugins/)，或者您可以自己轻松创建一个。

如果您有兴趣扩展 Sketch，那么您在这里就对了。在这里，我们展示 Sketch 可扩展性文档的概要以及如何快速构建您的第一个 Sketch 插件。

如果您只想使用现有插件，请参阅 [Plugin Directory](https://sketchapp.com/extensions/plugins/)。

### 你可以用插件做什么?

在 Sketch 中, 插件可做一个用户可以做的任何事情(甚至更多!). 例如:

* 根据复杂规则选择文档中的图层
* 操纵图层属性
* 创建新图层
* 以所有支持的格式导出资源
* 与用户交互（询问输入，显示输出）
* 从外部文件和 Web 服务获取数据
* 与剪贴板交互
* 操纵 Sketch 的环境（编辑指南，缩放等...）
* 通过调用插件中的菜单选项自动执行现有功能
* [设计规范](https://github.com/utom/sketch-measure)
* [内容生成](https://github.com/timuric/Content-generator-sketch-plugin)
* [透视变换](https://github.com/jamztang/MagicMirror)

查看 Sketch 插件的最简单方法是通过 [Plugin Directory](https://sketchapp.com/extensions/plugins/)。您可以浏览有用的插件，安装它们以试用它们，并了解如何为您自己的设计方案扩展 Sketch。

### 写一个扩展

我们创建了一个小工具链，这使得创建新插件变得非常容易。它非常适合[初学者](/guides/first-plugin)，你也可以找到现有的插件[示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples/)。

扩展程序是用 JavaScript 编写的。Sketch 提供了一个类似 REPL 的小型控制台，您可以在开始构建插件之前尝试使用它的 API。

### 扩展的想法

许多出色的 Sketch 功能社区创意最好作为插件实现，而不是作为核心产品的一部分。这样用户就可以通过安装正确的插件来选择他们想要的功能。Sketch 团队在 [plugin-request 存储库](https://github.com/sketchplugins/plugin-requests/issues)中的 Github issues 跟踪问题插件。如果您正在寻找一个很棒的插件来构建，请查看 issues。

## 下一步

* [你的第一个插件](/guides/first-plugin) - 尝试创建一个简单的Hello World插件。
* [扩展 API](/reference/) - 了解 Sketch 可扩展性 API。
* [扩展示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples/) - 您可以查看和构建的扩展示例列表。
* [开发者论坛](http://sketchplugins.com/) - 一个论坛，插件开发者分享他们对所有事情的知识。

### 帮助我们改进

如果您在文档中发现任何错误或遗漏，或者您希望我们覆盖或澄清某些内容，请[提交问题](https://github.com/BoltDoggy/SketchAPI-CN/issues)，我们会尝试修复它。当然，由于本网站上的所有内容都是开源的，您可以通过 [suggesting an edit on GitHub](https://github.com/BoltDoggy/SketchAPI-CN/) 来帮助我们改进它（每页底部还有一个“Improve this page”链接，以防您浏览网站时发现错误）。