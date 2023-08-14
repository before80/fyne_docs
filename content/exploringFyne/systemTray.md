+++
title = "系统托盘菜单"
date = 2023-08-14T08:55:10+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# System Tray Menu - 系统托盘菜单

https://developer.fyne.io/explore/systray

## 添加系统托盘菜单 Adding a System Tray menu

Since the v2.2.0 release Fyne has built in support for a system tray menu. This feature displays an icon on macOS, Windows and Linux computers and when tapped will pop out a menu as specified by the app.

​	自从 v2.2.0 版本开始，Fyne 已经内置了对系统托盘菜单的支持。这个功能在 macOS、Windows 和 Linux 计算机上显示一个图标，当点击图标时，会弹出一个菜单，菜单的内容由应用程序指定。

As this is a desktop specific feature we must first do a runtime check that the app is running in desktop mode. To do this, and get a reference to the desktop features, we do a Go type assertion:

​	由于这是一个桌面特定的功能，我们首先必须运行时检查应用程序是否在桌面模式下运行。为了做到这一点，并获取到桌面特性的引用，我们进行一个 Go 类型断言：

```go
	if desk, ok := a.(desktop.App); ok {
...
	}
```

If the `ok` variable is true then we can set up a menu using the standard Fyne menu API that you might have used in `Window.SetMainMenu` before.

​	如果 `ok` 变量为 true，那么我们可以使用标准的 Fyne 菜单 API 设置一个菜单，你可能在之前使用过 `Window.SetMainMenu`。

```go
		m := fyne.NewMenu("MyApp",
			fyne.NewMenuItem("Show", func() {
				log.Println("Tapped show")
			}))
		desk.SetSystemTrayMenu(m)
```

With this code added to the setup of your application you can run the app and see that it shows a Fyne icon in the system tray. When you tap it a menu will appear containing “Show” and “Quit”.

​	将这段代码添加到应用程序的设置中，你可以运行应用程序并看到它在系统托盘中显示了一个 Fyne 图标。当你点击它时，会出现一个菜单，其中包含“Show”和“Quit”。

The default icon is the Fyne logo, you can either fix this using [app metadata](https://developer.fyne.io/started/metadata) or by setting the app icon in `App.SetIcon` or for system tray directly using `desk.SetSystemTrayIcon`.

​	默认的图标是 Fyne 徽标，你可以使用[应用程序元数据](https://developer.fyne.io/started/metadata)进行修复，或者通过在 `App.SetIcon` 中设置应用程序图标，或者直接在系统托盘中使用 `desk.SetSystemTrayIcon` 进行设置。

## 管理窗口生命周期 Manage window lifecycle

By default a Fyne app will exit when you close all windows and this may not be what you want with a system tray app. To override the behaviour you can use the `Window.SetCloseIntercept` feature to override what happens when a window is closed. In the example below we hide the window instead of closing it by calling `Window.Hide()`. Add this before you show the window for the first time.

​	默认情况下，Fyne 应用程序在关闭所有窗口时退出，但这可能并不适用于系统托盘应用程序。要覆盖这种行为，你可以使用 `Window.SetCloseIntercept` 功能来覆盖窗口关闭时的操作。在下面的示例中，我们通过调用 `Window.Hide()` 隐藏窗口，而不是关闭窗口。在第一次显示窗口之前添加这个操作。

```go
	w.SetCloseIntercept(func() {
		w.Hide()
	})
```

The benefit of hiding a window is that you can simply show it again using `Window.Show()` which is much more efficient than creating a new window if the same content is needed a second time. We update the menu created earlier to show the window that was hidden above.

​	隐藏窗口的好处是，你可以使用 `Window.Show()` 简单地再次显示它，这比如果需要第二次显示相同的内容，创建一个新窗口要更高效。我们更新上面创建的菜单，以显示先前隐藏的窗口。

```go
	fyne.NewMenuItem("Show", func() {
		w.Show()
	}))
```

## 完整的应用程序 Complete app

That’s all there is to setting up a system tray menu with Fyne! The complete code for this tutorial is as follows.

​	这就是使用 Fyne 设置系统托盘菜单的全部内容！本教程的完整代码如下。

```go
package main

import (
	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/driver/desktop"
	"fyne.io/fyne/v2/widget"
)

func main() {
	a := app.New()
	w := a.NewWindow("SysTray")

	if desk, ok := a.(desktop.App); ok {
		m := fyne.NewMenu("MyApp",
			fyne.NewMenuItem("Show", func() {
				w.Show()
			}))
		desk.SetSystemTrayMenu(m)
	}

	w.SetContent(widget.NewLabel("Fyne System Tray"))
	w.SetCloseIntercept(func() {
		w.Hide()
	})
	w.ShowAndRun()
}
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/t0vnGbzoB3I" frameborder="0" allowfullscreen="" style="box-sizing: border-box;"></iframe>