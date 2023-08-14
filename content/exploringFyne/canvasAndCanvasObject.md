+++
title = "Canvas 和 CanvasObject"
date = 2023-08-14T08:52:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Canvas and CanvasObject

https://developer.fyne.io/explore/canvas

In Fyne a `Canvas` is the area within which an application is drawn. Each window has a canvas which you can access with `Window.Canvas()` but usually you will find functions on `Window` that avoid accessing the canvas.

​	在 Fyne 中，`Canvas` 是应用程序绘制区域。每个窗口都有一个画布，您可以使用 `Window.Canvas()` 来访问它，但通常您会在 `Window` 上找到避免访问画布的函数。

Everything that can be drawn in Fyne is a type of `CanvasObject`. The example here opens a new window and then shows different types of primitive graphical element by setting the content of the window canvas. There are many ways that each type of object can be customised as shown with the text and circle examples.

​	在 Fyne 中，可以绘制的每个内容都是 `CanvasObject` 的一种类型。此示例在新窗口中打开，并通过设置窗口画布的内容来显示不同类型的原始图形元素。正如文本和圆形示例所示，每种类型的对象都有许多自定义方式。

As well as changing the content shown using `Canvas.SetContent()` it is possible to change the content that is currently visible. If, for example, you change the `FillColour` of a rectangle you can request a refresh of this existing component using `rect.Refresh()`.

​	除了使用 `Canvas.SetContent()` 更改显示的内容外，还可以更改当前可见的内容。例如，如果更改了矩形的 `FillColour`，则可以使用 `rect.Refresh()` 请求刷新此现有组件。

```go
package main

import (
	"image/color"
	"time"

	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/canvas"
)

func main() {
	myApp := app.New()
	myWindow := myApp.NewWindow("Canvas")
	myCanvas := myWindow.Canvas()

	blue := color.NRGBA{R: 0, G: 0, B: 180, A: 255}
	rect := canvas.NewRectangle(blue)
	myCanvas.SetContent(rect)

	go func() {
		time.Sleep(time.Second)
		green := color.NRGBA{R: 0, G: 180, B: 0, A: 255}
		rect.FillColor = green
		rect.Refresh()
	}()

	myWindow.Resize(fyne.NewSize(100, 100))
	myWindow.ShowAndRun()
}
```

We can draw many different drawing elements in the same way, such as circle and text.

​	我们可以以同样的方式绘制许多不同的绘图元素，比如圆形和文本。

```go
func setContentToText(c fyne.Canvas) {
	green := color.NRGBA{R: 0, G: 180, B: 0, A: 255}
	text := canvas.NewText("Text", green)
	text.TextStyle.Bold = true
	c.SetContent(text)
}

func setContentToCircle(c fyne.Canvas) {
	red := color.NRGBA{R: 0xff, G: 0x33, B: 0x33, A: 0xff}
	circle := canvas.NewCircle(color.White)
	circle.StrokeWidth = 4
	circle.StrokeColor = red
	c.SetContent(circle)
}
```

## Widget

A `fyne.Widget` is a special type of canvas object that has interactive elements associated with it. In widgets the logic is separate from the way that it looks (also called the `WidgetRenderer`).

​	`fyne.Widget` 是一种特殊类型的画布对象，与之相关联的是交互元素。在小部件中，逻辑与外观分离（也称为 `WidgetRenderer`）。

Widgets are also types of `CanvasObject` and so we can set the content of our window to a single widget. See how we create a new `widget.Entry` and set it as the content of the window in this example.

​	小部件也是 `CanvasObject` 的类型，因此我们可以将窗口的内容设置为单个小部件。在此示例中，我们如何创建一个新的 `widget.Entry` 并将其设置为窗口的内容。

```go
package main

import (
	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/widget"
)

func main() {
	myApp := app.New()
	myWindow := myApp.NewWindow("Widget")

	myWindow.SetContent(widget.NewEntry())
	myWindow.ShowAndRun()
}
```