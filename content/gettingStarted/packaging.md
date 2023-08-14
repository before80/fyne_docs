+++
title = "打包"
date = 2023-08-14T08:38:58+08:00
weight = 8
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Packaging - 打包

https://developer.fyne.io/started/packaging

## 桌面应用的打包 - Packaging for Desktop

------

Packaging a graphical app for distribution can be complex. Graphical applications typically have icons and metadata associated with them as well as specific formats required to integrate with each environment. Windows executables need embedded icons, macOS apps are bundles and with Linux there are various metadata files that should get installed. What a hassle!

​	为了分发图形应用程序，打包可能会变得复杂。图形应用程序通常需要与它们关联的图标和元数据，以及与每个环境集成所需的特定格式。Windows可执行文件需要嵌入的图标，macOS应用程序是捆绑包，Linux则有各种应安装的元数据文件。真是麻烦！

Thankfully the “fyne” app has a “package” command that can handle this automatically. Just specifying the target OS and any required metadata (such as icon) will generate the appropriate package. The icon conversion will be done automatically for .icns or .ico so just provide a .png file :). All you need is to have the application already built for the target platform…

​	幸运的是，“fyne”应用程序具有一个“package”命令，可以自动处理这个问题。只需指定目标操作系统和任何所需的元数据（例如图标），就可以生成适当的软件包。图标转换将自动完成 .icns 或 .ico 文件，所以只需提供一个 .png 文件 :). 您只需将应用程序已经构建为目标平台…

```bash
go install fyne.io/fyne/v2/cmd/fyne@latest
fyne package -os darwin -icon myapp.png
```

If you’re using an older version of Go (<1.16), you should install fyne using `go get`

​	如果您正在使用较旧版本的 Go (<1.16)，您应该使用 `go get` 安装 fyne

```bash
go get fyne.io/fyne/v2/cmd/fyne
fyne package -os darwin -icon myapp.png
```

Will create myapp.app, a complete bundle structure for distribution to macOS users. You could then build the linux and Windows versions too…

​	将创建 myapp.app，这是一个完整的捆绑包结构，可分发给 macOS 用户。然后您可以构建 Linux 和 Windows 版本...

```bash
fyne package -os linux -icon myapp.png
fyne package -os windows -icon myapp.png
```

These commands will create:

​	这些命令将创建：

- myapp.tar.gz that contains a folder structure starting at usr/local/ that a Linux user could expand to the root of their system.
- myapp.tar.gz，其中包含从 usr/local/ 开始的文件夹结构，Linux 用户可以将其展开到其系统的根目录。
- myapp.exe (after the second build, which is required for a windows package) will have the icon and app metadata embedded.
- myapp.exe（在第二次构建之后，这是 windows 软件包所需的），将嵌入图标和应用程序元数据。

If you just want to install the desktop app on your computer then you can make use of the helpful install subcommand. For example to install your current application system wide you could simply execute the following:

​	如果您只想在计算机上安装桌面应用程序，那么您可以使用有用的 install 子命令。例如，要在系统范围内安装当前应用程序，您只需执行以下操作：

```bash
fyne install -icon myapp.png
```

All of these commands also support a default icon file of `Icon.png` so that you can avoid typing the parameter for each execution. Since Fyne 2.1 there is also a [metadata file](https://developer.fyne.io/started/metadata) where you can set default options for your project.

​	所有这些命令还支持默认的图标文件 `Icon.png`，以便您可以避免为每次执行键入参数。自从 Fyne 2.1 版本以来，还有一个[元数据文件](https://developer.fyne.io/started/metadata)，您可以在其中为项目设置默认选项。

Of course you can still run your applications using the standard Go tools if you prefer.

​	当然，如果您喜欢，仍然可以使用标准的 Go 工具来运行您的应用程序。

<iframe width="560" height="315" src="https://www.youtube.com/embed/zIV3Pv5QbeM" frameborder="0" allowfullscreen="" style="box-sizing: border-box;"></iframe>