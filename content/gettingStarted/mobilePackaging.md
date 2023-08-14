+++
title = "移动应用打包"
date = 2023-08-14T08:39:10+08:00
weight = 9
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Mobile Packaging - 移动应用打包

https://developer.fyne.io/started/mobile

## 打包移动应用 - Packaging Mobile Apps

------

Your Fyne app code will work out of the box as mobile apps, just as it did for desktop. However it is a little more complex to package the code for distribution. This page will explore the process to do just that to get your app on iOS and Android.

​	您的 Fyne 应用代码将在移动应用中自动运行，就像在桌面应用中一样。但是，将代码打包以进行分发要稍微复杂一些。本页面将探讨如何将您的应用程序打包以便将其发布到 iOS 和 Android 平台上。

Firstly you will need some more development tools installed for mobile packaging to complete. For Android builds you must have the Android SDK and NDK installed with appropriate environment set up so that the tools (such as `adb`) can be found on the command line. To build iOS apps you will need Xcode installed on your macOS computer as well as the command line tools optional package.

​	首先，您需要安装一些用于移动应用打包的开发工具。对于 Android 构建，您必须安装 Android SDK 和 NDK，并设置适当的环境，以便在命令行中可以找到工具（如 `adb`）。要构建 iOS 应用，您需要在 macOS 计算机上安装 Xcode，以及可选的命令行工具。

Once you have a working development environment the packaging is simple. To build an application for Android and iOS the following commands will do everything for you. Be sure to have a unique application identifier as it is unwise to change these after your first release.

​	一旦您拥有了工作的开发环境，打包过程就很简单了。要为 Android 和 iOS 构建应用程序，以下命令将为您完成一切。请确保拥有唯一的应用程序标识符，因为在第一次发布后更改这些标识符是不明智的。

```bash
fyne package -os android -appID com.example.myapp -icon mobileIcon.png
fyne package -os ios -appID com.example.myapp -icon mobileIcon.png
```

After these commands have completed (which may take some time on first compilation) you will see two new files in your directory, `myapp.apk` and `myapp.app`. You will see that the latter has the same name as a darwin application bundle - don’t get them confused as they will not work on the other platform.

​	在这些命令完成后（第一次编译可能需要一些时间），您会在目录中看到两个新文件，`myapp.apk` 和 `myapp.app`。请注意，后者与 darwin 应用程序捆绑包具有相同的名称 - 不要将它们弄混，因为它们在另一个平台上不起作用。

To install the android app on your phone or a simulator simply call:

​	要在手机或模拟器上安装 Android 应用，只需调用：

```bash
adb install myapp.apk
```

For iOS to install on device open Xcode and choose the “Devices and Simulators” menu item within the “Window” menu. Then find your phone and drag the `myapp.app` icon onto your app list.

​	要在 iOS 上安装到设备上，请打开 Xcode 并在“窗口”菜单中选择“Devices and Simulators”菜单项。然后找到您的手机，将 `myapp.app` 图标拖放到您的应用列表中。

If you want to install on a simulator make sure to package your application using `iossimulator` vs `ios`

​	如果您想要在模拟器上安装，请确保使用 `iossimulator` 而不是 `ios` 来打包您的应用程序

```bash
fyne package -os iossimulator -appID com.example.myapp -icon mobileIcon.png
```

Afterwards you can use the command line tools as follows:

​	之后，您可以使用以下命令行工具：

```bash
xcrun simctl install booted myapp.app
```