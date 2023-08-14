+++
title = "发布"
date = 2023-08-14T08:39:37+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

# Distribution - 分发

https://developer.fyne.io/started/distribution

## 分发到应用商店 - Distributing to App Stores

------

Packaging a graphical app as described in the [Packaging](https://developer.fyne.io/started/packaging) page provides a file or bundle that could be directly shared or distributed. However signing and uploading to app stores and market places is an additional step that requires platform-specific configuration, which we will cover in this page.

​	如 [打包](../packaging) 页面所述，将图形应用程序打包为文件或捆绑包后，可以直接共享或分发。但是，签名和上传到应用商店和市场需要额外的步骤，需要特定于平台的配置，我们将在本页面中介绍。

In each of these steps we will use a new tool that is part of the fyne command line utilities. The `fyne release` step handles the signing and preparation for each store, but the parameters vary per-platform, which we see below.

​	在以下每个步骤中，我们将使用 fyne 命令行工具中的一个新工具。`fyne release` 步骤处理了每个商店的签名和准备工作，但每个平台的参数各不相同，我们将在下面看到。

### macOS App Store（自 fyne 1.4.2 起） - macOS App Store (since fyne 1.4.2)

Prerequisites:

​	先决条件：

- Apple mac running macOS and Xcode
- 运行 macOS 和 Xcode 的 Apple Mac
- Apple Developer account
- Apple 开发者帐户
- Mac App Store application certificate
- Mac App Store 应用程序证书
- Mac App Store installer certificate
- Mac App Store 安装程序证书
- Apple Transporter app from App Store
- 从 App Store 下载 Apple Transporter 应用

1. Set up your app / version ready for a build to be uploaded at [AppStore Connect](https://appstoreconnect.apple.com/).
2. 在 [AppStore Connect](https://appstoreconnect.apple.com/) 上设置好您的应用/版本，准备好要上传构建。
3. Bundle the completed app for release:
4. 为发布准备好的完成的应用进行捆绑：

```bash
$ fyne release -appID com.example.myapp -appVersion 1.0 -appBuild 1 -category games
```

3. Drag the `.pkg` onto Transporter and tap “Deliver”.
4. 将 `.pkg` 拖到 Transporter 上，然后点击“Deliver”。
5. Go to back to the AppStore Connect website, choose your build for the release and submit for review.
6. 返回 AppStore Connect 网站，选择您要发布的构建版本，然后提交审核。

### Google Play Store (Android)

Prerequisites:

​	先决条件：

- Google Play Console account
- Google Play Console 帐户
- distribution keystore (creation instructions in [android docs](https://developer.android.com/studio/publish/app-signing))
- 分发密钥库（创建说明在 [android 文档](https://developer.android.com/studio/publish/app-signing) 中）
- Set up your app / version ready for build to be uploaded at [Google Play Console](https://play.google.com/apps/publish). Turn off “Play app signing” option as we manage it ourselves.
- 在 [Google Play Console](https://play.google.com/apps/publish) 上设置好您的应用/版本，准备好要上传构建。关闭“Play app signing”选项，因为我们自行管理签名。
- Bundle the completed app for release:
- 为发布准备好的完成的应用进行捆绑：

```bash
$ fyne release -os android -appID com.example.myapp -appVersion 1.0 -appBuild 1
```

3. Drag the `.apk` file into the build drop zone on the app version page in Play Console
4. 将 `.apk` 文件拖到 Play Console 上应用版本页面的构建放置区域。
5. Start rollout of new version.
6. 开始新版本的推出。

### iOS App Store (since fyne 1.4.1)

Prerequisites:

​	先决条件：

- Apple mac running macOS and Xcode
- 运行 macOS 和 Xcode 的 Apple Mac
- Apple Developer account
- Apple 开发者帐户
- iOS App Store distribution certificate
- iOS App Store 分发证书
- Apple Transporter app from App Store
- 从 App Store 下载 Apple Transporter 应用
- Set up your app / version ready for a build to be uploaded at [AppStore Connect](https://appstoreconnect.apple.com/).
- 在 [AppStore Connect](https://appstoreconnect.apple.com/) 上设置好您的应用/版本，准备好要上传构建。
- Bundle the completed app for release:
- 为发布准备好的完成的应用进行捆绑：

```bash
$ fyne release -os ios -appID com.example.myapp -appVersion 1.0 -appBuild 1
```

3. Drag the `.ipa` onto Transporter and tap “Deliver”.
4. 将 `.ipa` 拖到 Transporter 上，然后点击“Deliver”。
5. Go to back to the AppStore Connect website, choose your build for the release and submit for review.
6. 返回 AppStore Connect 网站，选择您要发布的构建版本，然后提交审核。