+++
title = "跨平台编译"
date = 2023-08-14T08:40:07+08:00
weight = 12
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Cross Compiling - 跨平台编译

https://developer.fyne.io/started/cross-compiling

## 为不同平台编译 - Compiling for different platforms

------

Cross compiling with Go is designed to be simple - we just set the environment variable `GOOS` for the target Operating System (and `GOARCH` if targeting a different architecture). Unfortunately when using native graphics calls the use of CGo in Fyne makes this a little harder.

​	在 Go 中进行交叉编译是很简单的 —— 我们只需为目标操作系统设置环境变量 `GOOS`（如果针对不同的体系结构还需要设置 `GOARCH`）。然而，当使用本地图形调用时，Fyne 中的 CGo 的使用使得这变得有点困难。

### 从开发计算机编译 - Compiling from a development computer

To cross-compile a Fyne application you will also have to set `CGO_ENABLED=1` which tells go to enable the C compiler (this is normally turned off when the target platform is different to the current system). Doing so unfortunately means that you must have a C compiler for the target platform that you are going to compile for. After installing the appropriate compilers you will also need to set the `CC` environment variable to tell Go which compiler to use.

​	要进行 Fyne 应用的交叉编译，您还必须设置 `CGO_ENABLED=1`，这告诉 Go 启用 C 编译器（当目标平台与当前系统不同时，这通常是关闭的）。这样做不幸的是意味着您必须为您要编译的目标平台拥有 C 编译器。在安装适当的编译器后，您还需要设置 `CC` 环境变量，以告诉 Go 使用哪个编译器。

There are many ways to install the required tools - and different tools that can be used. The configuration recommended by the Fyne developers is:

​	有很多方法可以安装所需的工具 —— 不同的工具可以使用。Fyne 开发人员推荐的配置如下：

| GOOS (target) | CC                               | provider          | download                                                    | 备注 notes                                                   |
| :------------ | :------------------------------- | :---------------- | :---------------------------------------------------------- | :----------------------------------------------------------- |
| `darwin`      | `o32-clang`                      | osxcross          | [from github.com](https://github.com/tpoechtrager/osxcross) | You will also need to install the macOS SDK (instructions at the download link) 您还需要安装 macOS SDK（下载链接中有说明） |
| `windows`     | `x86_64-w64-mingw64-gcc`         | mingw64           | package manager                                             | For macOS use [homebrew](https://brew.sh/) 对于 macOS，请使用 [homebrew](https://brew.sh/) |
| `linux`       | `gcc` or `x86_64-linux-musl-gcc` | gcc or musl-cross | [cygwin](https://www.cygwin.com/) or package manager        | musl-cross is available from [homebrew](https://brew.sh/) to provide the linux gcc. You will also need to install X11 and mesa headers for compilation. musl-cross 可从 [homebrew](https://brew.sh/) 获取，以提供 Linux gcc。您还需要安装 X11 和 mesa 标头以进行编译。 |

With the environment variables above set you should be able to compile in the usual manner. If further errors occur it is likely to be due to missing packages. Some target platforms require additional libraries or headers to be installed for the compilation to succeed.

​	设置了上述环境变量后，您应该能够以通常的方式进行编译。如果出现进一步的错误，很可能是由于缺少软件包。一些目标平台需要安装附加的库或标头，以便编译成功。

### 使用虚拟环境 Using a virtual environment

As a Linux system is able to cross compile to macOS and Windows easily it can be simpler to use a virtualised environment when you are not developing from Linux. Docker images are a useful tool for a complex build configuration and this works for Fyne as well. There are different tools that can be used. The tool recommended by the Fyne developers is [fyne-cross](https://github.com/fyne-io/fyne-cross). It has been inspired by [xgo](https://github.com/karalabe/xgo) and uses a [docker image](https://hub.docker.com/r/fyneio/fyne-cross) built on top of the [golang-cross](https://github.com/docker/golang-cross) image, that includes the MinGW compiler for windows, and a macOS SDK, along with the Fyne requirements.

​	由于 Linux 系统能够轻松地交叉编译到 macOS 和 Windows，所以在您不是从 Linux 进行开发时，使用虚拟化环境可能更简单。Docker 镜像是一个复杂构建配置的有用工具，它同样适用于 Fyne。有不同的工具可以使用。Fyne 开发人员推荐的工具是 [fyne-cross](https://github.com/fyne-io/fyne-cross)。它受到 [xgo](https://github.com/karalabe/xgo) 的启发，并使用构建在 [golang-cross](https://github.com/docker/golang-cross) 镜像之上的 [docker 镜像](https://hub.docker.com/r/fyneio/fyne-cross)，该镜像包括了用于 Windows 的 MinGW 编译器，以及 macOS SDK 和 Fyne 的要求。

fyne-cross allows to build binaries and create distribution packages for the following targets:

​	fyne-cross 允许构建二进制文件并为以下目标创建分发包：

| GOOS    | GOARCH |
| :------ | :----- |
| darwin  | amd64  |
| darwin  | 386    |
| linux   | amd64  |
| linux   | 386    |
| linux   | arm64  |
| linux   | arm    |
| windows | amd64  |
| windows | 386    |
| android | amd64  |
| android | 386    |
| android | arm64  |
| android | arm    |
| ios     |        |
| freebsd | amd64  |
| freebsd | arm64  |

> Note: iOS compilation is supported only on darwin hosts.
>
> 注意：仅在 darwin 主机上支持 iOS 编译。

#### 要求 Requirements

- go >= 1.13
- docker

#### 安装 Installation

You can install `fyne-cross` using the following command (requires Go 1.16 or later):

​	您可以使用以下命令安装 `fyne-cross`（需要 Go 1.16 或更高版本）：

```bash
go install github.com/fyne-io/fyne-cross@latest
```

For earlier versions of Go, you need to use the following command instead:

​	对于较早版本的 Go，您需要使用以下命令：

```bash
go get github.com/fyne-io/fyne-cross
```

#### 用法 Usage

```bash
fyne-cross <command> [options]

The commands are:

	darwin        Build and package a fyne application for the darwin OS
	linux         Build and package a fyne application for the linux OS
	windows       Build and package a fyne application for the windows OS
	android       Build and package a fyne application for the android OS
	ios           Build and package a fyne application for the iOS OS
	freebsd       Build and package a fyne application for the freebsd OS
	version       Print the fyne-cross version information

Use "fyne-cross <command> -help" for more information about a command.
```

#### 通配符 Wildcards

The `arch` flag support wildcards in case want to compile against all supported GOARCH for a specified GOOS

​	`arch` 标志支持通配符，以防需要针对指定 GOOS 的所有支持的 GOARCH 进行编译

Example:

​	示例：

```bash
fyne-cross windows -arch=*
```

is equivalent to

等同于

```bash
fyne-cross windows -arch=amd64,386
```

#### 示例 Example

The example below cross compile and package the [fyne examples application](https://github.com/fyne-io/examples)

​	以下示例交叉编译和打包 [fyne 示例应用](https://github.com/fyne-io/examples)

```bash
git clone https://github.com/fyne-io/examples.git
cd examples
```

##### 编译和打包主要示例应用 - Compile and package the main example app

```bash
fyne-cross linux
```

> Note: by default fyne-cross will compile the package into the current dir.
>
> The command above is equivalent to: `fyne-cross linux .`
>
> 注意：默认情况下，fyne-cross 将将软件包编译到当前目录。
>
> 上面的命令等同于：`fyne-cross linux .`

##### 编译和打包特定示例应用 - Compile and package a particular example app

```bash
fyne-cross linux -output bugs ./cmd/bugs
```