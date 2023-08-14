+++
title = "usingThePreferencesAPI"
date = 2023-08-14T08:54:48+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

# Using the Preferences API - 使用偏好设置 API

https://developer.fyne.io/explore/preferences

Storing user configurations and values is a common task for application developers, but implementing it across multiple platforms can be tedious and time-consuming. To make it easier, Fyne has an API for storing values on the filesystem in a clean and understandable way while the complex parts are handled for you.

​	存储用户配置和值是应用程序开发人员常见的任务，但在多个平台上实现它可能会很繁琐和耗时。为了使其更容易，Fyne 提供了一个 API，可以以干净和可理解的方式将值存储在文件系统中，同时复杂的部分由 API 处理。

Lets start with the setup of the API. It is part of the [Preferences](https://pkg.go.dev/fyne.io/fyne/v2?tab=doc#Preferences) interface where storage and loading functions exist for values of Bool, Float, Int and String. They each consist of three different functions, one for loading, one loading with a fallback value and lastly, one for storing values. An example of the three functions and their behaviour can be seen below for the String type:

​	我们从 API 的设置开始。它是 [Preferences](https://pkg.go.dev/fyne.io/fyne/v2?tab=doc#Preferences) 接口的一部分，其中包含用于 Bool、Float、Int 和 String 类型的值的存储和加载函数。它们各自由三个不同的函数组成，一个用于加载，一个带有默认值的加载，最后一个用于存储值。下面是 String 类型的三个函数及其行为示例：

```go
// String looks up a string value for the key
// String 查找键的字符串值
String(key string) string
// StringWithFallback looks up a string value and returns the given fallback if not found
// StringWithFallback 查找字符串值，并在找不到时返回给定的默认值
StringWithFallback(key, fallback string) string
// SetString saves a string value for the given key
// SetString 为给定的键保存字符串值
SetString(key string, value string)
```

These functions can be accessed through the created application variable and calling the `Preferences()` method on. Please note that it is necessary to create the apps with a unique ID (usually like a reversed url). This means that the application will need to be created using `app.NewWithID()` to have its own place to store values. It can roughly be used like the example below:

​	通过创建的应用程序变量，并调用其上的 `Preferences()` 方法，可以访问这些函数。请注意，必须使用唯一的 ID（通常类似于反转的 URL）来创建应用程序。这意味着应用程序需要使用 `app.NewWithID()` 来创建自己的存储值的地方。它可以大致如下所示：

```go
a := app.NewWithID("com.example.tutorial.preferences")
[...]
a.Preferences().SetBool("Boolean", true)
number := a.Preferences().IntWithFallback("ApplicationLuckyNumber", 21)
expression := a.Preferences().String("RegularExpression")
[...]
```

To show this, we are going to build a simple little app that always closes after a set amount of time. This timeout should be user changeable and applied on the next start of the application.

​	为了展示这一点，我们将构建一个简单的小应用程序，它在设置的时间后总是关闭。这个超时应该是用户可更改的，并在下次启动应用程序时应用。

Let us start by creating a variable called `timeout` that will be used to store time in the form of `time.Duration`.

​	让我们从创建一个名为 `timeout` 的变量开始，用于以 `time.Duration` 的形式存储时间。

```go
var timeout time.Duration
```

Then we could create a select widget to let the user select the timeout from a couple pre-defined strings and then multiplying the timeout by the number of seconds that the string relates to. Lastly, the `"AppTimeout"` key is used to set the string value to the selected one.

​	然后，我们可以创建一个选择小部件，让用户从一些预定义的字符串中选择超时时间，然后将超时时间乘以字符串所关联的秒数。最后，使用 `"AppTimeout"` 键将字符串值设置为所选值。

```go
timeoutSelector := widget.NewSelect([]string{"10 seconds", "30 seconds", "1 minute"}, func(selected string) {
    switch selected {
    case "10 seconds":
        timeout = 10 * time.Second
    case "30 seconds":
        timeout = 30 * time.Second
    case "1 minute":
        timeout = time.Minute
    }

    a.Preferences().SetString("AppTimeout", selected)
})
```

Now we want to grab the set value and if none exists, we want to have a fallback that sets the timeout to the shortest one possible to save the user time when waiting. This can be done by setting the selected value of `timeoutSelector` to the loaded value or the fallback if that happens to be the case. By doing it this way, the code inside the select widget will run for that specific value.

​	现在，我们想要获取设置的值，并且如果没有值存在，我们想要一个默认值，将超时设置为最短的一个，以节省用户等待时间。这可以通过将 `timeoutSelector` 的选定值设置为加载的值或如果情况是这样的话，使用默认值。通过这种方式，选择小部件内的代码将针对特定的值运行。

```go
timeoutSelector.SetSelected(a.Preferences().StringWithFallback("AppTimeout", "10 seconds"))
```

The last part will just be to have a function that starts in a separate goroutine and tells the application to quit after the selected timeout.

​	最后一部分只是一个在单独的 goroutine 中启动的函数，它告诉应用程序在选定的超时后退出。

```go
go func() {
    time.Sleep(timeout)
    a.Quit()
}()
```

In the end, the resulting code should look something like this:

​	最终，生成的代码应该类似于这样：

```go
package main

import (
    "time"

    "fyne.io/fyne/v2/app"
    "fyne.io/fyne/v2/widget"
)

func main() {
    a := app.NewWithID("com.example.tutorial.preferences")
    w := a.NewWindow("Timeout")

    var timeout time.Duration

    timeoutSelector := widget.NewSelect([]string{"10 seconds", "30 seconds", "1 minute"}, func(selected string) {
        switch selected {
        case "10 seconds":
            timeout = 10 * time.Second
        case "30 seconds":
            timeout = 30 * time.Second
        case "1 minute":
            timeout = time.Minute
        }

        a.Preferences().SetString("AppTimeout", selected)
    })

    timeoutSelector.SetSelected(a.Preferences().StringWithFallback("AppTimeout", "10 seconds"))

    go func() {
        time.Sleep(timeout)
        a.Quit()
    }()

    w.SetContent(timeoutSelector)
    w.ShowAndRun()
}
```