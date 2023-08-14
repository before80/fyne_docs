+++
title = "窗口处理"
date = 2023-08-14T08:38:37+08:00
weight = 6
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Window Handling - 窗口处理

https://developer.fyne.io/started/windows

Windows are created using `App.NewWindow()` and need to be shown using the `Show()` function. The helper method `ShowAndRun()` on `fyne.Window` allows you to show your window and run the application at the same time.

​	窗口通过`App.NewWindow()`创建，并需要使用`Show()`函数进行显示。在`fyne.Window`上的辅助方法`ShowAndRun()`允许您同时显示窗口并运行应用程序。

By default a window will be the right size to show its content by checking the `MinSize()` function (more on that in later examples). You can set a larger size by calling the `Window.Resize()` method. Into this is passed a `fyne.Size` which contains a width and height using device independent pixels (meaning that it will be the same across different devices), for example to make a window square by default we could:

​	默认情况下，窗口的大小将足够以显示其内容，通过检查`MinSize()`函数（后面的示例将会更详细说明）。您可以通过调用`Window.Resize()`方法来设置较大的大小。您需要传递一个`fyne.Size`，其中包含设备无关像素的宽度和高度（这意味着在不同设备上是相同的）。例如，要创建一个默认为正方形的窗口，可以这样：

```go
	w.Resize(fyne.NewSize(100, 100))
```

Be aware that the desktop environment may have constraints that cause windows to be smaller than requested. Mobile devices will typically ignore this as they are only displayed at full-screen.

​	请注意，桌面环境可能会有限制，导致窗口比请求的尺寸要小。移动设备通常会忽略这一点，因为它们通常只显示全屏。

If you wish to show a second window you must only call the `Show()` function. It can also be helpful to split `Window.Show()` from `App.Run()` if you want to open multiple windows when your application starts. The example below shows how to load two windows when starting.

​	如果您希望显示第二个窗口，只需调用`Show()`函数即可。如果您希望在应用程序启动时打开多个窗口，将`Window.Show()`与`App.Run()`分开可能会很有帮助。下面的示例展示了如何在启动时加载两个窗口。

```go
package main

import (
	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/widget"
)

func main() {
	a := app.New()
	w := a.NewWindow("Hello World")

	w.SetContent(widget.NewLabel("Hello World!"))
	w.Show()

	w2 := a.NewWindow("Larger")
	w2.SetContent(widget.NewLabel("More content"))
	w2.Resize(fyne.NewSize(100, 100))
	w2.Show()

	a.Run()
}
```

The above application will exit when both windows are closed. If your app is arranged so one window is main and the others are accessory views you can set one window to be “master” so that the app exits if that window is closed. To do this use the `SetMaster()` function on `Window`.

​	上述应用程序将在两个窗口都关闭时退出。如果您的应用程序中的一个窗口是主窗口，而其他窗口是辅助视图，您可以设置一个窗口为“主窗口”，这样如果关闭了该窗口，应用程序将退出。要实现这一点，使用`Window`上的`SetMaster()`函数。

Windows can be created at any time, we could change the code above so that the content of the second window (`w2`) is a button that opens a new window. You could also load windows from more complex workflows, but be careful because new windows will normally appear above the current active content.

​	窗口可以在任何时候创建，我们可以更改上面的代码，使第二个窗口（`w2`）的内容为一个打开新窗口的按钮。您还可以从更复杂的工作流程加载窗口，但要小心，因为新窗口通常会出现在当前活动内容的上方。

```go
	w2.SetContent(widget.NewButton("Open new", func() {
		w3 := a.NewWindow("Third")
		w3.SetContent(widget.NewLabel("Third"))
		w3.Show()
	}))
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/nM54LsMyBQY" frameborder="0" allowfullscreen="" style="box-sizing: border-box;"></iframe>