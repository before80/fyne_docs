+++
title = "数据绑定"
date = 2023-08-14T08:55:22+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Data Binding - 数据绑定

https://developer.fyne.io/explore/binding

Data binding was introduced in Fyne v2.0.0 and makes it easier to connect many widgets to a data source that will update over time. the `data/binding` package has many helpful bindings that can manage most standard types that will be used in an application. A data binding can be managed using the binding API (for example `NewString`) or it can be connected to an external item of data like (`BindInt(*int)`).

​	数据绑定在 Fyne v2.0.0 中引入，使得将许多小部件连接到随时间更新的数据源变得更加容易。`data/binding` 包提供了许多有用的绑定，可以管理大多数在应用程序中使用的标准类型。数据绑定可以使用绑定 API 进行管理（例如 `NewString`），也可以连接到外部数据项（如 `BindInt(*int)`）。

Widgets that support binding typically have a `...WithData` constructor to set up the binding when creating the widget. You can also call `Bind()` and `Unbind()` to manage the data of an existing widget. The following example shows how you can manage a `String` data item that is bound to a simple `Label` widget.

​	支持绑定的小部件通常具有带有 `...WithData` 构造函数，用于在创建小部件时设置绑定。您还可以调用 `Bind()` 和 `Unbind()` 来管理现有小部件的数据。下面的示例展示了如何管理一个绑定到简单的 `Label` 小部件的 `String` 数据项。

```go
package main

import (
	"time"

	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/data/binding"
	"fyne.io/fyne/v2/widget"
)

func main() {
	a := app.New()
	w := a.NewWindow("Hello")

	str := binding.NewString()
	go func() {
		dots := "....."
		for i := 5; i >= 0; i-- {
			str.Set("Count down" + dots[:i])
			time.Sleep(time.Second)
		}
		str.Set("Blast off!")
	}()

	w.SetContent(widget.NewLabelWithData(str))
	w.ShowAndRun()
}
```

You can find out more in the [data binding](https://developer.fyne.io/binding/) section of this site.

​	您可以在本网站的[数据绑定](https://developer.fyne.io/binding/)部分中了解更多信息。