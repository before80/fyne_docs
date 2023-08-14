+++
title = "测试"
date = 2023-08-14T08:38:46+08:00
weight = 7
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Testing - 测试

https://developer.fyne.io/started/testing

## 测试图形应用程序 - Testing Graphical Apps

------

Part of a good test suite is being able to quickly write tests and run them on a regular basis. Fyne’s API is designed to make testing applications easy. By separating component logic from its rendering definition we can load applications without actually displaying them and test the functionality completely.

​	一个良好的测试套件的一部分是能够快速编写测试并定期运行它们。Fyne的API旨在使测试应用程序变得容易。通过将组件逻辑与其渲染定义分开，我们可以在不实际显示应用程序的情况下加载它们并完全测试其功能。

### 示例 Example

We can demonstrate unit testing by extending our [Hello World](https://developer.fyne.io/started/hello) app to include space for users to input their name to be greeted. We start by updating the user interface to have two elements, a `Label` for the greeting and an `Entry` for the name input. We display them, one above another, using `container.NewVBox` (a vertical box container). The updated user interface code will look as follows:

​	我们可以通过将我们的[Hello World](https://developer.fyne.io/started/hello)应用程序扩展为包含用户输入姓名的空间来演示单元测试。我们首先更新用户界面，添加了两个元素，一个用于问候的`Label`和一个用于输入姓名的`Entry`。我们使用`container.NewVBox`（一个垂直盒子容器）将它们显示在一个上面，一个下面。更新后的用户界面代码如下所示：

```go
func makeUI() (*widget.Label, *widget.Entry) {
	return widget.NewLabel("Hello world!"),
		widget.NewEntry()
}

func main() {
	a := app.New()
	w := a.NewWindow("Hello Person")

	w.SetContent(container.NewVBox(makeUI()))
	w.ShowAndRun()
}
```

To test this input behaviour we create a new file (with a name ending `_test.go` to mark it as tests) that defines a `TestGreeter` function.

​	为了测试这个输入行为，我们创建一个新文件（以`_test.go`结尾的名称，以标记它为测试文件），其中定义了一个`TestGreeter`函数。

```go
 package main

import (
	"testing"
)

func TestGreeting(t *testing.T) {
}
```

We can add an intial test that verifies the initial state, to do this we test the `Text` field of the `Label` that is returned from `makeUI` and error the test if it is not correct. Add the following code to your test method:

​	我们可以添加一个初始测试来验证初始状态，为此，我们测试从`makeUI`返回的`Label`的`Text`字段，并在它不正确时报告测试错误。将以下代码添加到测试方法中：

```go
	out, in := makeUI()

	if out.Text != "Hello world!" {
		t.Error("Incorrect initial greeting")
	}
```

This test will pass - next we add to the test to validate the greeter. We use the Fyne `fyne.io/fyne/v2/test` package which assists in test scenarios, calling `test.Type` to simulate user input. The following test code will check that the output updates when the user’s name is input (be sure to add the import as well):

​	这个测试将通过 —— 接下来，我们添加测试以验证问候者。我们使用Fyne的`fyne.io/fyne/v2/test`包，该包有助于测试场景，我们调用`test.Type`来模拟用户输入。以下测试代码将检查当用户输入姓名时输出是否更新（确保还要添加导入）：

```go
	test.Type(in, "Andy")
	if out.Text != "Hello Andy!" {
		t.Error("Incorrect user greeting")
	}
```

You can run all of these tests using `go test .` - just like any other tests. Doing so you will now see a failure - because we did not add the greeter logic. Update the `makeUI` function to the following code:

​	您可以使用`go test .`运行所有这些测试 - 就像运行其他测试一样。这样做，您现在会看到一个失败 - 因为我们没有添加问候者逻辑。将`makeUI`函数更新为以下代码：

```go
func makeUI() (*widget.Label, *widget.Entry) {
	out := widget.NewLabel("Hello world!")
	in := widget.NewEntry()

	in.OnChanged = func(content string) {
		out.SetText("Hello " + content + "!")
	}
	return out, in
}
```

Doing so you will see that the tests now pass. You can also run the full application (using `go run .`) and see the greeting update as you enter a name in the `Entry` field. Notice also that these tests all run without displaying a window or stealing your mouse - this is another benefit of the Fyne unit testing setup.

​	这样，您会看到测试现在通过了。您还可以运行完整的应用程序（使用`go run .`），并在`Entry`字段中输入姓名时查看问候语的更新。还要注意，这些测试在不显示窗口或占用鼠标的情况下运行 - 这是Fyne单元测试设置的另一个好处。

<iframe width="560" height="315" src="https://www.youtube.com/embed/AVIQn1areC4" frameborder="0" allowfullscreen="" style="box-sizing: border-box;"></iframe>