+++
title = "编译选项"
date = 2023-08-14T08:55:35+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Compile Options - 编译选项

https://developer.fyne.io/explore/compiling

## 编译标签 Build tags

Fyne will typically configure your application appropriately for the target platform by selecting the driver and configuration. The following build tags are supported and can help in your development. For example if you wish to simulate a mobile application whilst running on a desktop computer you could use the following command:

​	Fyne 通常会根据目标平台选择驱动程序和配置，适当地配置您的应用程序。以下编译标签受到支持，可以在您的开发中提供帮助。例如，如果您希望在桌面计算机上运行时模拟移动应用程序，可以使用以下命令：

```bash
go run -tags mobile main.go
```

| 标签 Tag          | 描述 Description                                             |
| :---------------- | :----------------------------------------------------------- |
| `gles`            | Force use of embedded OpenGL (GLES) instead of full OpenGL. This is normally controlled by the target device and not normally needed. 强制使用嵌入式 OpenGL（GLES）而不是完整的 OpenGL。这通常由目标设备控制，通常不需要。 |
| `hints`           | Display developer hints for improvements or optimisations. Running with `hints` will log when your application does not follow material design or other recommendations. 显示开发者提示以进行改进或优化。使用 `hints` 运行时，当您的应用程序不遵循材料设计或其他建议时，将会记录日志。 |
| `mobile`          | This tag runs an application in a simulated mobile window. Useful when you want to preview your app on a mobile platform without compiling and installing to the device. 此标签在模拟的移动窗口中运行应用程序。当您希望在移动平台上预览应用程序而无需编译和安装到设备时，这非常有用。 |
| `no_native_menus` | This flag is specifically for macOS and indicates that the application should not use the macOS native menus. Instead menus will be displayed inside the application window. Most useful for testing an application on macOS to simulate the behavior on Windows or Linux. 此标记专门针对 macOS，指示应用程序不应使用 macOS 原生菜单。取而代之的是，菜单将显示在应用程序窗口内。对于在 macOS 上测试应用程序以模拟在 Windows 或 Linux 上的行为非常有用。 |