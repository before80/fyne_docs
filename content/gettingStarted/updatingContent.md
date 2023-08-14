+++
title = "更新内容"
date = 2023-08-14T08:38:26+08:00
weight = 5
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Updating Content - 更新内容

https://developer.fyne.io/started/updating

## 更新图形界面中的内容 Updating content in your GUI

------

Having completed the [hello world](https://developer.fyne.io/started/hello) tutorial or other examples you will have created a basic user interface. In this page we see how the content of a GUI can be updated from your code.

​	在完成了[Hello World](https://developer.fyne.io/started/hello)教程或其他示例后，您将创建了一个基本的用户界面。在本页中，我们将看到如何从您的代码中更新图形界面的内容。

The first step is to assign the widget you want to update to a variable. In the hello world tutorial we passed `widget.NewLabel` directly into `SetContent()`, to update it we change that to two different lines, such as:

​	第一步是将要更新的小部件分配给一个变量。在Hello World教程中，我们直接将`widget.NewLabel`传递给`SetContent()`，要更新它，我们将其更改为两行不同的代码，如下所示：

```go
	clock := widget.NewLabel("")
	w.SetContent(clock)
```

Once the content has been assigned to a variable we can call functions like `SetText("new text")`. For our example we will set the content of our label to the current time, with the help of `Time.Format`.

​	一旦内容被分配给一个变量，我们可以调用`SetText("new text")`等函数进行更新。在我们的示例中，我们将标签的内容设置为当前时间，使用了`Time.Format`函数来帮助格式化时间。

```go
	formatted := time.Now().Format("Time: 03:04:05")
	clock.SetText(formatted)
```

That is all we need to do to change content of a visible item (see below for the full code). However, we can go further and update content on a regular basis.

​	这就是我们改变可见项目内容所需的全部内容（完整的代码见下文）。然而，我们还可以进一步，定期更新内容。

## 在后台运行- Running in the background

Most applications will need to have processes that run in the background, for example downloading data or responding to events. To simulate this we will extend the code above to run every second.

​	大多数应用程序需要在后台运行进程，例如下载数据或响应事件。为了模拟这一点，我们将扩展上面的代码以每秒运行一次。

Like with most go code we can create a goroutine (using the `go` keyword) and run our code there. If we move the text update code to a new function it can be called on initial display as well as on a timer for regular updating. By combining a goroutine and the `time.Tick` inside a for loop we can update the label every second.

​	与大多数Go代码一样，我们可以创建一个协程（使用`go`关键字）并在其中运行我们的代码。如果我们将文本更新代码移到一个新函数中，它既可以在初始显示时调用，也可以在定时器上调用以进行定期更新。通过将协程和`time.Tick`结合在一个循环内，我们可以每秒更新标签。

```go
	go func() {
		for range time.Tick(time.Second) {
			updateTime(clock)
		}
	}()
```

It is important to place this code before `ShowAndRun` or `Run` calls because they will not return until the application closes. With all of this together the code will run and update the user interface each second, creating a basic clock widget. The full code is as follows:

​	重要的是将此代码放置在`ShowAndRun`或`Run`调用之前，因为它们将在应用程序关闭之前不会返回。将所有这些代码组合在一起，代码将每秒运行一次并更新用户界面，从而创建一个基本的时钟小部件。完整的代码如下：

```go
package main

import (
	"time"

	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/widget"
)

func updateTime(clock *widget.Label) {
	formatted := time.Now().Format("Time: 03:04:05")
	clock.SetText(formatted)
}

func main() {
	a := app.New()
	w := a.NewWindow("Clock")

	clock := widget.NewLabel("")
	updateTime(clock)

	w.SetContent(clock)
	go func() {
		for range time.Tick(time.Second) {
			updateTime(clock)
		}
	}()
	w.ShowAndRun()
}
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/h2ZOdTA3ew4" frameborder="0" allowfullscreen="" style="box-sizing: border-box;"></iframe>