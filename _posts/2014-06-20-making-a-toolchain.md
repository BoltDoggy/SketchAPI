---
title: 制作工具链，而不仅仅是一个工具
---

Sketch is a great tool for humans, but sometimes (especially in larger teams), we want to let the robots get in on the action too.

Making apps, websites, even icons is all about iteration: make it, try it, debug it, refine it, rinse and repeat.

Whilst we can’t automate the creative stuff, there are some steps in each of these iterations that are just drudge work. Exporting assets at a particular set of sizes and formats is an obvious example here, but maybe you are also supposed to extract certain measurements each time, or fire off certain exports in an email, or submit them to source control, or perform some other simple jobs.

We’ve done our best to make things like exporting a pain-free process from within Sketch itself, but even so, these jobs are often ripe for a more complete form of automation.

Wouldn’t it be nice if you could write scripts that are able to parse sketch files, extracting data and exporting images then doing things with the results?

Wouldn’t it be even nicer if these scripts could run unattended, maybe in response to your document changing, perhaps on a server where Sketch isn’t installed?

In some cases, there may be existing chains of tools and scripts in place as part of your (or your company’s) workflow. Wouldn’t it be nice if you could integrate Sketch with these?

This is why we made [sketchtool](http://www.sketchapp.com/tool/).

## 它不是什么

First, it’s probably sensible to say what sketchtool doesn’t do.

For now, at least, sketchtool cannot create new Sketch documents, nor can it modify existing ones.

We may support a broader range of features at a later date, but for now, sketchtool is all about extracting things from existing documents.

## 这是什么

Essentially sketchtool lets you do three categories of thing:

- get a list of pages, artboards, slices
- export pages, artboards, slices or arbitrary layers
- get a full description of the entire document

### 清单

Given a sketch document called Test.sketch, you can list the pages in it like this:

```
sketchtool list pages Test.sketch
```

This will output a JSON record describing the name, id and bounds of each page.

Similarly

```
sketchtool list artboards Test.sketch
sketchtool list slices Test.sketch
```

does the same for the artboards and slices (and exported layers) in a document.

### 出口

Getting information from a document is all very well, but what you probably want is to export images.

To extract everything that has been marked for export in a document, you can do:

```
sketchtool export layers Test.sketch
```

Similarly:

```
sketchtool export pages Test.sketch
sketchtool export artboards Test.sketch
sketchtool export slices Test.sketch
```

You can also modify the output using various options.

The `--output` option lets you specify the output folder for the export.

The `--formats` option lets you specify a list of formats to use for the export (e.g. “svg,png”).

The `--scales` option lets you specify the scales to export at (x1, x2 etc).

The `--items` option lets you list just one or more items to export, by name or id.

Options such as `--save-for-web`, `--overwriting`, `--compact`, `--trimmed`, and `--compression` also let you tailor the output.

### 倾倒文件

目前使用 sketchtool 的最后一个选项是 dump命令：

```
sketchtool dump Test.sketch
```

这将输出文档的完整JSON描述。

该描述非常详细，因此输出变得很快，很快。 它还公开了很多关于底层模型的内部细节，因此它将来会发生变化，如果你在脚本中使用这个命令，你应该为它们可能会破坏一天做好准备。

在许多情况下，如果使用 list 命令无法获取您尝试提取的信息，那么您的下一个最佳选择可能是使用 SVG 导出，并解析它，因为它是一种稳定的格式。

如果这不起作用，您可以使用 dump 命令获取所需的信息。

### 未来

将来，我们可能会扩展 sketchtool 来做更多事情。

如果您有功能请求，错误报告或仅讲述您正在使用它做的很酷的事情，请与我们取得联系。

您可以下载最新版本的 sketchtool [here](http://www.sketchapp.com/tool/)。

(For comments, I’m [@samdeane](https://twitter.com/samdeane) on Twitter)