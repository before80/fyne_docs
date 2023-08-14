+++
title = "运行 Fyne 演示"
date = 2023-08-14T08:37:50+08:00
weight = 3
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Run Fyne Demo - 运行 Fyne 演示

https://developer.fyne.io/started/demo

If you want to see the Fyne toolkit in action before you start to code your own application, you can see our [demo app](https://github.com/fyne-io/fyne/tree/master/cmd/fyne_demo).

​	如果在开始编写你自己的应用程序之前，你想要在实际操作中看到 Fyne 工具包的效果，你可以查看我们的[演示应用程序](https://github.com/fyne-io/fyne/tree/master/cmd/fyne_demo)。

### 运行 Running

If you want to, you can run the demo directly using the following command (requires Go 1.16 or later):

​	如果你愿意，你可以直接使用以下命令来运行演示（需要 Go 1.16 或更新版本）：

```bash
go run fyne.io/fyne/v2/cmd/fyne_demo@latest
```

For earlier versions of Go, you need to use the following command instead:

​	对于早期版本的 Go，你需要使用以下命令：

```bash
go run fyne.io/fyne/v2/cmd/fyne_demo
```

By browsing the different tabs of the app you can see all the features of Fyne toolkit.

​	通过浏览应用程序的不同选项卡，你可以看到 Fyne 工具包的所有功能。

### 安装 Installing

It is possible to install the app as a graphical application like all others on your computer. We have the helpful `fyne` tool to do this for you. First you will need to install the tool:

​	你可以将这个应用程序安装为计算机上的其他所有图形应用程序一样。我们有一个名为 `fyne` 的有用工具可以帮助你完成这个过程。首先，你需要安装这个工具：

```bash
go install fyne.io/fyne/v2/cmd/fyne@latest
```

After that you can simply package and install the demo app:

​	之后，你只需简单地对演示应用程序进行打包和安装：

```bash
fyne get fyne.io/fyne/v2/cmd/fyne_demo
```

After this step you can find “Fyne Demo” in your app launcher.

​	在完成这个步骤后，你可以在应用启动器中找到 "Fyne 演示"。

### 探索代码 Exploring the code

If you are interested in any of the features you should check out the [source code](https://github.com/fyne-io/fyne/tree/master/cmd/fyne_demo) or join one of the [community channels](https://fyne.io/#contact).

​	如果你对任何功能感兴趣，你可以查看[源代码](https://github.com/fyne-io/fyne/tree/master/cmd/fyne_demo)或加入其中的[社区频道](https://fyne.io/#contact)。