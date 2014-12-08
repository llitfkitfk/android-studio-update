Gradle Plugin User Guide
=====================

1. [Introduction](#introduction-)
	1. [Goals of the new Build System](#goals-of-the-new-build-system-) 
	2. [Why Gradle?](#why-gradle-)
2. [Requirements](#requirements-)
3. [Basic Project](#basic-project-)
	1. Simple build files
	2. Project Structure
		1. Configuring the Structure 
	3. Build Tasks
		1. General Tasks 
		2. Java project tasks
		3. Android tasks
	4. Basic Build Customization
		1. Manifest entries
		2. Build Types
		3. Signing Configurations
		4. Running ProGuard

4. [Dependencies, Android Libraries and Multi-project setup](#dependencies-android-libraries-and-multi-project-setup-)
	1. Dependencies on binary packages
		1. Local packages
		2. Remote artifacts
	2. Multi project setup
	3. Library projects
		1. Creating a Library Project
		2. Differences between a Project and a Library Project
		3. Referencing a Library
		4. Library Publication
5. [Testing](#testing-)
	1. Basics and Configuration
	2. Running tests
	3. Testing Android Libraries
	4. Test reports
		1. Single projects
		2. Multi-projects reports
	5. Lint support
6. [Build Variants](#build-variants-)
	1. Product flavors
	2. Build Type + Product Flavor = Build Variant
	3. Product Flavor Configuration
	4. Sourcesets and Dependencies
	5. Building and Tasks
	6. Testing
	7. Multi-flavor variants
7. [Advanced Build Customization](#advanced-build-customization-)
	1. Build options
		1. Java Compilation options
		2. aapt options
		3. dex options
	2. Manipulating tasks
	3. BuildType and Product Flavor property reference
	4. Using sourceCompatibility 1.7



#Introduction [\^](#gradle-plugin-user-guide)

This documentation is for the Gradle plugin version 0.9. Earlier versions may differ due to non-compatible we are introducing before 1.0.

```
这个文档的gradle 插件版本是0.9
在1.0版本之前的早期版本因为不兼容可能会有些不同
```

##Goals of the new Build System [\^](#gradle-plugin-user-guide)

The goals of the new build system are:

* Make it easy to reuse code and resources
* Make it easy to create several variants of an application, either for multi-apk distribution or for different flavors of an application
* Make it easy to configure, extend and customize the build process
* Good IDE integration

```
新构造系统的目标:
-> 方便复用代码和资源
-> 方便创建不同的应用，多包分布式或者是不同风格的应用
-> 方便配置，扩展以及自定义构造过程
-> 良好的IDE集成
```

##Why Gradle? [\^](#gradle-plugin-user-guide)

Gradle is an advanced build system as well as an advanced build toolkit allowing to create custom build logic through plugins.

```
Gradle时一种先进的构建系统，同时也是先进的构建工具包，允许用户通过插件来创建自定义的构建逻辑
```

Here are some of its features that made us choose Gradle:

* Domain Specific Language (DSL) to describe and manipulate the build logic
* Build files are Groovy based and allow mixing of declarative elements through the DSL and using code to manipulate the DSL elements to provide custom logic.
* Built-in dependency management through Maven and/or Ivy.
* Very flexible. Allows using best practices but doesn’t force its own way of doing things.
* Plugins can expose their own DSL and their own API for build files to use.
* Good Tooling API allowing IDE integration

```
下面是让我们选择Gradle的一些功能：
-> 领域特定语言(DSL)用来描述和操作构建逻辑 
-> 构建文件是基于Groovy的，允许通过DSL混合声明的元素以及使用代码来操作DSL元素并提供自定义的逻辑
-> 通过 Maven 以及/或者 Ivy 内置依赖管理
-> 非常灵活，允许用户使用最佳实践并不强制Gradle统一的方式
-> 良好的工具API允许IDE集成
```

#Requirements [\^](#gradle-plugin-user-guide)

* Gradle 1.10 or 1.11 or 1.12 with the plugin 0.11.1
* SDK with Build Tools 19.0.0. Some features may require a more recent version.

```
-> 插件版本是0.11.1的 Gradle 1.10 或者1.11 或者1.12，
-> 构建工具是19.0.0的SDK. 一些功能可能需要较新的版本

```

#Basic Project [\^](#gradle-plugin-user-guide)

A Gradle project describes its build in a file called build.gradle located in the root folder of the project.

```
Gradle根目录下的build.gradle文件是Gradle项目的构建文件
```

##Simple build files [\^](#gradle-plugin-user-guide)

The most simple Java-only project has the following build.gradle:

```
最简单的java项目的构建文件如下：
```

```
apply plugin: 'java'
```

This applies the Java plugin, which is packaged with Gradle. The plugin provides everything to build and test Java applications.


```
Gradle打包就是应用了java插件，这个插件提供了所有的东西来构建测试java应用
```

The most simple Android project has the following build.gradle:

```
最简单的 android项目用以下的build.gradle构建：
```

```
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.11.1'
    }
}

apply plugin: 'android'

android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"
}
```

There are 3 main areas to this Android build file:

**buildscript { ... }** configures the code driving the build.
In this case, this declares that it uses the Maven Central repository, and that there is a classpath dependency on a Maven artifact. This artifact is the library that contains the Android plugin for Gradle in version 0.11.1
Note: This only affects the code running the build, not the project. The project itself needs to declare its own repositories and dependencies. This will be covered later.

Then, the **android** plugin is applied like the Java plugin earlier.

Finally, **android { ... }** configures all the parameters for the android build. This is the entry point for the Android DSL.
By default, only the compilation target, and the version of the build-tools are needed. This is done with the **compileSdkVersion** and **buildtoolsVersion** properties. The compilation target is the same as the **target** property in the project.properties file of the old build system. This new property can either be assigned a int (the api level) or a string with the same value as the previous **target** property.

```
android 构建文件有三部分
-> buildscript { ... } 配置了驱动构建的代码
-> 在这种情况下，该声明 用了maven的中央库，并且配置了一个含有版本为0.11.1的android Gradle插件类路径。
-> 注意：这样只会影响运行构建过程，对整个项目没有影响，该项目本身需要声明自己的仓库与依赖，稍后会提到。

-> 然后，android 插件的应用类似于上边的java插件

-> 最后，android {...} 配置了所有构建android的参数，这是android DSL的入口
-> 默认情况下，只需要有编译目标(compileSdkVersion)以及构建工具(buildtoolsVersion)的版本。
-> 其中编译目标类似于旧的构建系统的project.properties里面的target属性；整个新属性可以使一个整数(API的级别)也可以是一个同值的string字符串
```

**Important:** You should only apply the **android** plugin. Applying the **java** plugin as well will result in a build error.

```
值得一提： 用户应当应用 android插件，如果应用java插件的话会引起构建错误```

**Note:** You will also need a local.properties file to set the location of the SDK in the same way that the existing SDK requires, using the **sdk.dir** property.
Alternatively, you can set an environment variable called **ANDROID_HOME**. There is no differences between the two methods, you can use the one you prefer.

```
注意：用户也需要一个local.properties文件来设置SDK的路径：sdk.dir；
	或者，用户可以设置环境变量ANDROID_HOME。
	这两者没有任何区别，用户可以自行选择。

```

##Project Structure [\^](#gradle-plugin-user-guide)

##Build Tasks [\^](#gradle-plugin-user-guide)

##Basic Build Customization [\^](#gradle-plugin-user-guide)

#Dependencies, Android Libraries and Multi-project setup [\^](#gradle-plugin-user-guide)

#Testing [\^](#gradle-plugin-user-guide)

#Build Variants [\^](#gradle-plugin-user-guide)

#Advanced Build Customization [\^](#gradle-plugin-user-guide)