---
title: Publishing Plugins
summary: How to publish a plugin for the first time and how to publish updates
permalink: /guides/publishing-plugins/
redirect_from: /introduction/updating-plugins/
order: 150
---

Sketch 插件被列在 GitHub 仓库中。该文档介绍了怎样将插件发布到那里以及如何让 Sketch 接收到你插件的更新。

## 第一次发布

Sketch 插件列在 GitHub 仓库中：[https://github.com/sketchplugins/plugin-directory](https://github.com/sketchplugins/plugin-directory).

要将插件添加到列表中，请提交一个包含插件信息的 PR。一旦它被合并后，你的插件将展示在这： [https://sketchapp.com/extensions/plugins/](https://sketchapp.com/extensions/plugins/)

如果你使用 `skpm`，那么第一次使用 `skpm publish` 发布插件时，它将自动为你创建相应的 PR。

## 发布更新

从 Sketch v45 版本开始，Sketch 提供了一种官方支持的机制，用于更新应用程序中的插件。

如果你的插件已经有了自己内置的更新机制，我们建议你换作使用新系统。这将改善用户体验，因为用户可以在应用程序的首选项选项卡里管理已经安装了的所有插件。

启动时我们会检查所有已安装插件的更新，如果有的话，我们会在 Sketch 的窗口上显示一个徽章，点击它时会跳转到应用程序的首选项，在这里他们可以更新他们的插件。

现在 Sketch 仅允许用户更新至最新版本。在未来的版本中 Sketch 可以提供额外的选项给用户，以选择要下载和安装哪个插件版本。

你有两种方式来选择性加入该更新机制：

### 1. 使用 `skpm`

通过执行 `skpm publish`, 它将自动为你发布插件的更新版本，并且会确保 Sketch 能够获取到它。

### 2. 手动操作

在插件包中包含的 `manifest.json` 文件里有一个附加条目，你需要定义该文件以便更新工作。

该条目称为 `appcast`, 它是一个字符串，指定了 appcast 文件的 URL。

#### `appcast.xml` 文件

appcast 文件包含插件更新的相关信息，例如可用更新的版本以及可以从哪里下载这些更新。Sketch 会下载该文件以确定是否有可用的插件更新。

Appcast 符合 [Sparkle 文档](https://sparkle-project.org/documentation/) 和 [发布更新页面](https://sparkle-project.org/documentation/publishing/#publishing-an-update) 中描述的 Sparkle 定义的 appcast。对于 Sketch 插件，仅支持 .zip 文件作为附件。

最小和最大系统版本不是指用于运行插件时的操作系统版本。究竟如何在以后的 Sketch 版本中使用它们仍然待定。

下面的 Appcast 示例列出了该插件的三种不同版本。每个版本都有自己的下载链接和简短的说明文本。

```xml
<?xml version="1.0" encoding="utf-8"?>
<rss version="2.0" xmlns:sparkle="http://www.andymatuschak.org/xml-namespaces/sparkle"  xmlns:dc="http://purl.org/dc/elements/1.1/">
  <channel>
    <title>Hello World Sketch Test Plugin</title>
    <link>http://sparkle-project.org/files/sparkletestcast.xml</link>
    <description>Brilliant Hello World Plugin</description>
    <language>en</language>
      <item>
        <title>Version 1.1</title>
        <description>
          <![CDATA[
            <ul>
              <li>Minor update v1.1</li>
            </ul>
          ]]>
        </description>
        <enclosure url="https://brillianthello.sketchplugins.com/files/HelloWorldSketchPluginTestv11.zip" sparkle:version="1.1" />
      </item>
      <item>
        <title>Version 1.2</title>
        <description>
          <![CDATA[
            <ul>
            <li>Minor update v1.2</li>
            </ul>
          ]]>
        </description>
        <enclosure url="https://brillianthello.sketchplugins.com/files/HelloWorldSketchPluginTestv12.zip" sparkle:version="1.2" />
      </item>
      <item>
        <title>Version 2.0</title>
        <description>
          <![CDATA[
            <ul>
            <li>Major update v2.0</li>
            </ul>
          ]]>
        </description>
        <enclosure url="https://brillianthello.sketchplugins.com/files/HelloWorldSketchPluginTestv20.zip" sparkle:version="2.0" />
      </item>
  </channel>
</rss>
```

### 在插件中实现启动和关闭方法

如果你的插件执行任何需要初始化的操作，则你应将启动处理程序作为插件的一部分实现。同样实现关闭处理程序也是如此，你应该在其中实现插件所需要的任何清理代码。你可能已经在使用这些事件了，但是通过插件更新，这比以往任何时候都重要。

插件更新时，会给正在被更新的版本发送一个关闭操作，给新版本发送一个启动操作。

例如，如果你的插件在 Sketch 中展示了一些用户界面元素，那么你应该在关闭处理程序中移除它们。这样的话，新插件就能够显示已删除所有旧用户界面元素更新后的用户界面组件。

插件所维护的任何持久性数据都是如此。调用关闭处理程序时，应将所有未保存的信息写入磁盘。

不要在稍后可以运行的启动处理程序当中包含代码。


### 疑难解答

如果你遵循了所有的步骤，但是插件仍然没有更新，那么试试这些方法：

- 删除位于 `~/Library/Application Support/com.bohemiancoding.sketch3/` 中的 `PluginsWarehouse` 文件夹。这是我们缓存插件下载的地方，如果你一直在测试 appcast 的不同版本，那么在这个地方很有可能有一些值得清理的旧东西。
- 确保下载的 ZIP 包中的 `manifest.json` 文件里的版本号和 appcast 中的版本号相匹配。如果 appcast 表示 ZIP 包含 v1.2，但实际的 ZIP 表示它是 v1.1，那么将无法正常安装。
