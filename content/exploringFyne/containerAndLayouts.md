+++
title = "容器和布局"
date = 2023-08-14T08:53:02+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Container and Layouts - 容器和布局

https://developer.fyne.io/explore/container

In the previous example we saw how to set a `CanvasObject` to the content of a `Canvas`, but it is not very useful to only show one visual element. To show more than one item we use the `Container` type.

​	在前面的示例中，我们了解了如何将 `CanvasObject` 设置为 `Canvas` 的内容，但仅显示一个视觉元素并不是很有用。为了显示多个项目，我们使用 `Container` 类型。

As the `fyne.Container` also is a `fyne.CanvasObject`, we can set it to be the content of a `fyne.Canvas`. In this example we create 3 text objects and then place them in a container using the `container.NewWithoutLayout()` function. As there is no layout set we can move the elements around like you see with `text2.Move()`.

​	由于 `fyne.Container` 也是 `fyne.CanvasObject`，因此我们可以将其设置为 `fyne.Canvas` 的内容。在这个示例中，我们创建了3个文本对象，然后使用 `container.NewWithoutLayout()` 函数将它们放入一个容器中。由于没有设置布局，我们可以像在 `text2.Move()` 中看到的那样移动元素。

```go
package main

import (
	"image/color"

	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/canvas"
	"fyne.io/fyne/v2/container"
	//"fyne.io/fyne/v2/layout"
)

func main() {
	myApp := app.New()
	myWindow := myApp.NewWindow("Container")
	green := color.NRGBA{R: 0, G: 180, B: 0, A: 255}

	text1 := canvas.NewText("Hello", green)
	text2 := canvas.NewText("There", green)
	text2.Move(fyne.NewPos(20, 20))
	content := container.NewWithoutLayout(text1, text2)
	// content := container.New(layout.NewGridLayout(2), text1, text2)

	myWindow.SetContent(content)
	myWindow.ShowAndRun()
}
```

A `fyne.Layout` implements a method for organising items within a container. By uncommenting the `container.New()` line in this example you alter the container to use a grid layout with 2 columns. Run this code and try resizing the window to see how the layout automatically configures the contents of the window. Notice also that the manual position of `text2` is ignored by the layout code.

​	`fyne.Layout` 实现了一种在容器中组织项目的方法。通过在此示例中取消注释 `container.New()` 行，您可以更改容器以使用具有 2 列的网格布局。运行此代码并尝试调整窗口的大小，以查看布局如何自动配置窗口的内容。还请注意，布局代码会忽略 `text2` 的手动位置。

To see more you can check out the [`Layout list`](https://developer.fyne.io/explore/layouts).

​	要了解更多信息，请查看 [`布局列表`](https://developer.fyne.io/explore/layouts)。