+++
title = "Application and RunLoop"
date = 2023-08-14T08:38:07+08:00
weight = 4
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Application and RunLoop

https://developer.fyne.io/started/apprun

For a GUI application to work it needs to run an event loop (sometimes called a runloop) that processes user interactions and drawing events. In Fyne this is started using the `App.Run()` or `Window.ShowAndRun()` functions. One of these must be called from the end of your setup code in the `main()` function.

​	为了使图形用户界面应用程序能够正常工作，它需要运行一个事件循环（有时称为运行循环），以处理用户交互和绘图事件。在 Fyne 中，可以使用 `App.Run()` 或 `Window.ShowAndRun()` 函数来启动这个事件循环。其中之一必须在 `main()` 函数的设置代码末尾进行调用。

An application should only have one runloop and so you should only call `Run()` once in your code. Calling it a second time will cause errors.

​	一个应用程序应该只有一个运行循环，所以在你的代码中只能调用一次 `Run()`。第二次调用将会导致错误。

```go
package main

import (
	"fmt"

	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/widget"
)

func main() {
	myApp := app.New()
	myWindow := myApp.NewWindow("Hello")
	myWindow.SetContent(widget.NewLabel("Hello"))

	myWindow.Show()
	myApp.Run()
	tidyUp()
}

func tidyUp() {
	fmt.Println("Exited")
}
```

For desktop runtimes an app can be quit directly by calling `App.Quit()` (mobile apps do not support this) - normally not needed in developer code. An application will also quit once all the windows are closed. See also that functions executed after `Run()` will not be called until the application exits.

​	对于桌面应用运行时，可以通过调用 `App.Quit()` 直接退出应用程序（移动应用不支持此功能） —— 在开发者代码中通常不需要。当所有窗口关闭后，应用程序也将退出。同时注意，在调用 `Run()` 后执行的函数将在应用程序退出时才会被调用。