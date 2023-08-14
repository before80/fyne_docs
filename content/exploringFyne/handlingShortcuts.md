+++
title = "为应用程序添加快捷方式"
date = 2023-08-14T08:54:19+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Adding Shortcuts to an App - 为应用程序添加快捷方式

https://developer.fyne.io/explore/shortcuts

Shortcuts are common tasks that can be triggered by keyboard combinations or context menus. Shortcuts, much like keyboard events, can be attached to a focused element or registered on the `Canvas` to always be available in a `Window`.

​	快捷方式是常见的任务，可以通过键盘组合键或上下文菜单触发。快捷键与键盘事件类似，可以附加到具有焦点的元素，也可以在 `Canvas` 上注册，以在 `Window` 中始终可用。

## 在 Canvas 中注册 - Registering with a Canvas

There are many standard shortcuts defined (such as `fyne.ShortcutCopy`) which are connected to standard keyboard shortcuts and right-click menus. The first step to adding a new `Shortcut` is to define the shortcut. For most uses this will be a keyboard triggered shortcut, which is a desktop extension. To do this we use `desktop.CustomShortcut`, for example to use the Tab key and Control modifier you might do the following:

​	已经定义了许多标准的快捷方式（如 `fyne.ShortcutCopy`），这些快捷方式与标准的键盘快捷键和右键菜单相关联。添加新的 `Shortcut` 的第一步是定义快捷键。对于大多数情况，这将是一个由键盘触发的快捷键，即桌面扩展。我们可以使用 `desktop.CustomShortcut` 来实现这一点，例如，要使用 Tab 键和 Control 修饰键，可以这样做：

```go
	ctrlTab    := &desktop.CustomShortcut{KeyName: fyne.KeyTab, Modifier: fyne.KeyModifierControl}
	ctrlAltTab := &desktop.CustomShortcut{KeyName: fyne.KeyTab, Modifier: fyne.KeyModifierControl | fyne.KeyModifierAlt}
```

Notice that this shortcut can be re-used so you could attach it to menus or other items as well. For this example we want it to be always available, so we register it with our window’s `Canvas` as follows:

​	请注意，此快捷键可以被重复使用，因此您也可以将其附加到菜单或其他项目上。在此示例中，我们希望它始终可用，因此我们将其注册到我们窗口的 `Canvas` 中，如下所示：

```go
	ctrlTab := &desktop.CustomShortcut{KeyName: fyne.KeyTab, Modifier: fyne.KeyModifierControl}
	w.Canvas().AddShortcut(ctrlTab, func(shortcut fyne.Shortcut) {
		log.Println("We tapped Ctrl+Tab")
	})
	w.Canvas().AddShortcut(ctrlAltTab, func(shortcut fyne.Shortcut) {
		log.Println("We tapped Ctrl+Alt+Tab")
	})
```

As you can see there are two parts to registering a shortcut in this way - passing the shortcut definition and also a callback function. If the user types the keyboard shortcut then the function will be called and the output printed.

​	正如您所看到的，通过这种方式注册快捷方式有两个部分 - 传递快捷方式定义以及回调函数。如果用户键入了键盘快捷键，那么函数将被调用并打印输出。

## 向 Entry 添加快捷方式 - Adding shortcuts to an Entry

It can also be helpful to have a shortcut apply only when the current item is focused. This approach can be used for any focusable widget, and is managed by extending that widget and adding a `TypedShortcut` handler. This is much like adding key handlers, except the value passed in will be a `fyne.Shortcut`.

​	当仅在当前项目获得焦点时才应用快捷方式也可能是有帮助的。此方法可用于任何可获得焦点的小部件，并通过扩展该小部件并添加 `TypedShortcut` 处理程序来进行管理。这与添加键处理程序非常相似，不同之处在于传递的值将是 `fyne.Shortcut`。

```go
type myEntry struct {
	widget.Entry
}

func (m *myEntry) TypedShortcut(s fyne.Shortcut) {
	if _, ok := s.(*desktop.CustomShortcut); !ok {
		m.Entry.TypedShortcut(s)
		return
	}

	log.Println("Shortcut typed:", s)
}
```

From the excerpt above you can see how a `TypedShortcut` handler might be implemented. Inside this function you should check whether the shortcut is of the custom type used earlier. If the shortcut is a standard one it’s a good idea to call the original shortcut handler (if the widget had one). With those checks done you can compare the shortcut with the various types you are handling (if there are multiple).

​	从上面的摘录中，您可以看到如何实现 `TypedShortcut` 处理程序。在此函数内，您应该检查快捷方式是否是之前使用的自定义类型。如果快捷方式是标准的，则最好调用原始快捷方式处理程序（如果小部件有一个）。完成这些检查后，您可以将快捷方式与您正在处理的各种类型进行比较（如果有多个）。