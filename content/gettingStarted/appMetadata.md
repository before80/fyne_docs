+++
title = "应用元数据"
date = 2023-08-14T08:39:51+08:00
weight = 11
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# App Metadata - 应用元数据

https://developer.fyne.io/started/metadata

## 应用元数据 - App Metadata

Since release v2.1.0 of the `fyne` command we support a metadata file that allows you to store information about your application in the repository. This file is optional, but can help to avoid having to remember specific build parameters for each package and release command.

​	自 `fyne` 命令的版本 v2.1.0 发布以来，我们支持一个元数据文件，允许您在存储库中存储有关应用程序的信息。该文件是可选的，但可以帮助您避免记住每个包和发布命令的特定构建参数。

The file should be named `FyneApp.toml` in the directory where you run the `fyne` command (this is normally the `main` package). The contents of the file are as follows:

​	文件应该命名为 `FyneApp.toml`，位于您运行 `fyne` 命令的目录中（通常是 `main` 包所在的目录）。文件的内容如下所示：

```toml
Website = "https://example.com"

[Details]
Icon = "Icon.png"
Name = "My App"
ID = "com.example.app"
Version = "1.0.0"
Build = 1
```

The top portion of the file is metadata that will be used if you upload your app to the https://apps.fyne.io listing page, so it is optional. The [Details] section contains data about your application that are used in the release process by other app stores and operating systems. The fyne tool will use this file if it is found, many mandatory command parameters are not required if the metadata is present. You can still override these values by using command line parameters.

​	文件的顶部部分是元数据，如果您将应用上传到 [https://apps.fyne.io](https://apps.fyne.io/) 列表页面，将使用此元数据，因此它是可选的。[Details] 部分包含有关您的应用程序的数据，这些数据在其他应用商店和操作系统的发布过程中使用。如果找到此文件，fyne 工具将使用它，如果元数据存在，则许多强制性的命令参数不再需要。您仍然可以通过使用命令行参数来覆盖这些值。