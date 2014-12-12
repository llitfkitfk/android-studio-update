[Go support for Android](https://docs.google.com/document/d/1N3XyVkAP8nmWjASz8L_OjjnjVKxgeVBjIsTr5qIUcA4/edit)
===========
David Crawshaw
June 2014

#Abstract
We propose to introduce Go support for the Android platform. The focus will be on supporting games written in Go. The APIs will be those defined in the Android NDK.

```
理论：

我们建议把Go引入Android平台。
重心将会是游戏的开发。
这些API将在Android NDK里面定义。
```

#Background
Android is an operating system designed for running apps. An app relies on far more platform libraries and services than are provided by a traditional Unix operating system, which means a direct port of the Go runtime to Android without new APIs would not be particularly useful.

Providing a Go equivalent to the Android platform is intractable. The platform is written in Java and has a huge API surface. Any attempt to wrap these APIs in Go would give an undesirable result: manually built wrappers would lag in features, automatically generated wrappers would lead to ugly Go. And either way, it would be slow.

There is however, a subset of Android apps written against a much smaller C-based API surface provided in the Android NDK: Games. It is feasible to build Go support for Android providing the equivalent features found in the NDK.

```
背景：

Android是一个为运行app而设计的操作系统。
一个App依赖于大量传统Unix操作系统的平台库以及服务。这就意味着直接移植Go runtime到Android而没有新的APIS的话将不会有特别的用处。

提供等同于Android平台的Go是棘手的。
该平台是用Java编写的，并拥有庞大的API。
任何试图用Go封装这些API不会有什么好的结果：功能上手动封装会滞后，自动生成的封装会使Go变得很难堪。而且任何一种方式进展都会很缓慢。

然而，对于用基于C语言API的Android NDK编写Android应用的一个子集：游戏 来说，
建立用Go语言作为为Android提供NDK的支持是可行的。
```

#Proposal
During the Go 1.4 cycle, GOOS=android will be introduced to the Go repository, along with cgo support on Android (contributed by Elias Naur). Dalvik/ART-loadable .so files will be produced using the external linker provided in the Android NDK.

For the build dashboard, we will maintain a cross-compiling builder that runs the Go tool on a linux host and uses the adb tool to run test binaries on a stock Android device.

```
建议：

在 Go 1.4版本中，GOOS=android将会被引入到Go的代码库，以及在Android(由 Elias Naur贡献)添加cgo支持。
Dalvik/ART-loadable.so文件将使用在Android NDK提供的外部连接器中产生。
对于构建版面，我们将维护一个交叉编译构建项目在Linux主机上运行Go语言工具，并且实用adb工具在Android设备上运行测试二进制。

```

We will introduce a subrepository, go.mobile. It will house:

* Bindings for OpenGL, OpenSL, and OpenMAX as exported through the Android NDK. 
* A Java -> Go language binding generator. Given a Go package, this will let Java code call it, so game menu UIs can be built in the standard SDK. (As Go defines the binding, it also makes it possible to use the same code to bind to languages like Objective C.)
* Android Studio build system integration.

```
我们将推出子仓库go.mobile。它涵盖以下内容：

-> 封装 OpenGL, OpenSL以及OpenMAX作为出口来通过Android NDK
-> Java -> Go语言的绑定生成器：
	给定一个Go pakage，它可以被Java调用，以此游戏界面可以被构建用标准的sdk。(正如Go定义了封装，它也可以是用相同的代码来封装其他语言像Objective C.) (PS:一统天下的节奏啊)
-> 与Android Studio构建系统的集成
```

Binary releases will be provided after the project has stabilized.

```
二进制版本将会在项目稳定之后发布！！！
```
