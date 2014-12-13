Gradle Plugin User Guide
=====================

1. [Introduction](#introduction-)
	1. [Goals of the new Build System](#goals-of-the-new-build-system-) 
	2. [Why Gradle?](#why-gradle-)
2. [Requirements](#requirements-)
3. [Basic Project](#basic-project-)
	1. [Simple build files](#simple-build-files-)
	2. [Project Structure](#project-structure-)
		1. [Configuring the Structure](#configuring-the-structure-) 
	3. [Build Tasks](#build-tasks-)
		1. [General Tasks](#general-tasks-) 
		2. [Java project tasks](#java-project-tasks-)
		3. [Android tasks](#android-tasks-)
	4. [Basic Build Customization](#basic-build-customization-)
		1. [Manifest entries](#manifest-entries-)
		2. [Build Types](#build-types-)
		3. [Signing Configurations](#signing-configurations-)
		4. [Running ProGuard](#running-proguard-)

4. [Dependencies, Android Libraries and Multi-project setup](#dependencies-android-libraries-and-multi-project-setup-)
	1. [Dependencies on binary packages](#dependencies-on-binary-packages-)
		1. [Local packages](#local-packages-)
		2. [Remote artifacts](#remote-artifacts-)
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

The basic build files above expect a default folder structure. Gradle follows the concept of convention over configuration, providing sensible default option values when possible.

```
上述的基本构建文件时默认的文件夹结构。Gradle遵循常规优于配置，提供合理的默认选项的理念

```

The basic project starts with two components called “source sets”. The main source code and the test code. These live respectively in:

```
基本项目始于两部分：“source sets”。主代码和测试代码，分别放置在如下位置：
```

```
src/main/
src/androidTest/
```

Inside each of these folders exists folder for each source components.
For both the Java and Android plugin, the location of the Java source code and the Java resources:

```
里面的每一个文件夹都有源代码组件的文件夹
对于java和android插件，源代码与资源文件都保存在下边两个位置里：
```

```
java/
resources/
```

For the Android plugin, extra files and folders specific to Android:

```
对于android插件，额外的文件和文件夹具体是下边这些：
```

```
AndroidManifest.xml
res/
assets/
aidl/
rs/
jni/
```

**Note:** ```src/androidTest/AndroidManifest.xml``` is not needed as it is created automatically.

```
注意：src/androidTest/AndroidManifest.xml 虽然自动创建了，但是是不需要的
```

###Configuring the Structure [\^](#gradle-plugin-user-guide)

When the default project structure isn’t adequate, it is possible to configure it. According to the Gradle documentation, reconfiguring the sourceSets for a Java project can be done with the following:

```
当默认的项目结构不足够以应对需求时，也可以添加配置。根据 Gradle文档，重新配置java项目的sourceSets可以通过以下来实现：
```

```
sourceSets {
    main {
        java {
            srcDir 'src/java'
        }
        resources {
            srcDir 'src/resources'
        }
    }
}
```

**Note:** srcDir will actually add the given folder to the existing list of source folders (this is not mentioned in the Gradle documentation but this is actually the behavior).

```
注意：srcDir 将会添加给定的文件夹到已经存在的源文件夹的列表里(在Gradle文档里并没有提到，但是这确实是个事实)
```

To replace the default source folders, you will want to use srcDirs instead, which takes an array of path. This also shows a different way of using the objects involved:

```
要替代默认的源文件夹，用户可以配置srcDirs(参数是路径数组)来实现，这里展示了不同的实现方式：
```

```
sourceSets {
    main.java.srcDirs = ['src/java']
    main.resources.srcDirs = ['src/resources']
}
```


For more information, see the Gradle documentation on the Java plugin [here](http://gradle.org/docs/current/userguide/java_plugin.html).

```
更多详情，请查阅Gradle文档的java插件
``` 
[点击这里](http://gradle.org/docs/current/userguide/java_plugin.html)

The Android plugin uses a similar syntaxes, but because it uses its own sourceSets, this is done within the android object.
Here’s an example, using the old project structure for the main code and remapping the androidTest sourceSet to the tests folder:

```
Android插件使用类似的语法，但是因为它使用自己的 sourceSets,这是Android项目的使用方式。
下面的举例，使用旧的项目结构的main code重新映射androidTest	的sourceSet到tests文件夹
```

```
android {
    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDirs = ['src']
            resources.srcDirs = ['src']
            aidl.srcDirs = ['src']
            renderscript.srcDirs = ['src']
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
        }
	androidTest.setRoot('tests')  
    }
}
```

**Note:** because the old structure put all source files (java, aidl, renderscript, and java resources) in the same folder, we need to remap all those new components of the sourceSet to the same src folder.

**Note:** setRoot() moves the whole sourceSet (and its sub folders) to a new folder. This moves src/androidTest/* to tests/*
This is Android specific and will not work on Java sourceSets.

```
-> 注意：因为旧的项目结构把所有的源文件(java, aidl, render script和 java resources)放在同一个文件夹里，所以用户需要重新映射所有sourceSet的新组件到相同的src文件夹。

-> 注意：setRoot() 移动所有的sourceSet(以及它的子文件夹)到新的文件夹里，上面例子里是移动 src/androidTest/* 到 tests/*。

-> 这是android项目的具体配置，对于Java项目sourceSets不起作用
```

The ‘migrated’ sample shows this.


##Build Tasks [\^](#gradle-plugin-user-guide)

###General Tasks [\^](#gradle-plugin-user-guide)

Applying a plugin to the build file automatically creates a set of build tasks to run. Both the Java plugin and the Android plugin do this.
The convention for tasks is the following:

```
应用插件来自动创建一系列的构建任务。Java插件和Android插件都可以做到。
构建任务的规定如下：
```

* assemble
	* The task to assemble the output(s) of the project
* check
	* The task to run all the checks.
* build
	* This task does both assemble and check
* clean
	* This task cleans the output of the project

```
-> assemble	－组装项目输出的任务
-> check		－运行所有检查的任务
-> build			－做以上两种任务的任务
-> clean		－清除项目输出的任务
```

The tasks assemble, check and build don’t actually do anything. They are anchor tasks for the plugins to add actual tasks that do the work.

```
组装，检查以及构建任务实际上并不做任何事。
它们的功能是让插件添加实际的任务来工作。
```

This allows you to always call the same task(s) no matter what the type of project is, or what plugins are applied.
For instance, applying the findbugs plugin will create a new task and make check depend on it, making it be called whenever the check task is called.


```
这可以使用户用相同的任务不管是什么类型的项目或者用什么类型的插件。
比如说，应用findbugs插件将创建新的任务并检查依赖，使得它被调用每当检查任务被调用
```

From the command line you can get the high level task running the following command:

```
以下的命令用户可以获得高级别的任务：
```

```
gradle tasks
```

For a full list and seeing dependencies between the tasks run:

```
展示任务之间依赖的全部列表：
```

```
gradle tasks --all
```

**Note:** Gradle automatically monitor the declared inputs and outputs of a task.
Running the build twice without change will make Gradle report all tasks as UP-TO-DATE, meaning no work was required. This allows tasks to properly depend on each other without requiring unneeded build operations.

```
注意：Gradle自动监视任务声明的输入的输出。
没有变化的运行构建两次会使Gradle报告所有的任务是最新的(UP-TO-DATE),这意味着没有需要的多余工作。
这使得任务可以正确的依赖彼此，而无需不必要的构建操作。
```

###Java project tasks [\^](#gradle-plugin-user-guide)

The Java plugin creates mainly two tasks, that are dependencies of the main anchor tasks:

```
Java插件主要创建两个任务：
```

* assemble
	* jar
		* This task creates the output.
* check
	* test
		* This task runs the tests.

```
-> assemble	－jar	－创建输出的任务
-> check		－test	－运行测试的任务
```

The jar task itself will depend directly and indirectly on other tasks: classes for instance will compile the Java code.
The tests are compiled with testClasses, but it is rarely useful to call this as test depends on it (as well as classes).

In general, you will probably only ever call assemble or check, and ignore the other tasks.

```
jar任务自身会直接或者间接一览其他的任务：类的实例将编译java代码。
//TODO
这些测试用testClasses编译，但是它几乎是没用的来调用此作为测试依赖类
一般情况下，用户可能只能调用组装活着检查，而忽略了其他任务
```

You can see the full set of tasks and their descriptions for the Java plugin [here](http://gradle.org/docs/current/userguide/java_plugin.html).

```
用户可以看到Java插件的全套任务及其说明
```
[点击这里](http://gradle.org/docs/current/userguide/java_plugin.html)

###Android tasks [\^](#gradle-plugin-user-guide)

The Android plugin use the same convention to stay compatible with other plugins, and adds an additional anchor task:

* assemble
	* The task to assemble the output(s) of the project
* check
	* The task to run all the checks.
* connectedCheck
	* Runs checks that requires a connected device or emulator. they will run on all connected devices in parallel.
* deviceCheck
	* Runs checks using APIs to connect to remote devices. This is used on CI servers.
* build
	* This task does both assemble and check
* clean
	* This task cleans the output of the project

```
Android插件使用相同的规定来保持与其他插件的兼容，并且增加了一个额外的任务：

-> assemble		－组装项目输出任务
-> check			－检查任务
-> connectedCheck	－检查需要一个连接设备或者模拟器。它们可以并行运行在所有连接的设备上。
-> deviceCheck 	－检查连接使用APIs的远程设备。这通常在CI服务上使用。
-> build				－做组装和检查的任务
-> clean			－清除项目输出的任务
```

The new anchor tasks are necessary in order to be able to run regular checks without needing a connected device.
Note that build does not depend on deviceCheck, or connectedCheck.

```
新的任务是必要的，以便能够无需连接设备就能运行定期检查。
需要注意的是构建不依赖于deviceCheck，或connectedCheck。
```

An Android project has at least two outputs: a debug APK and a release APK. Each of these has its own anchor task to facilitate building them separately:

* assemble
	* assembleDebug
	* assembleRelease

```
一个android项目至少有两个输出：一个调试APK和一个正式APK。
每一种都有自己的任务，以便它们单独构建：

-> assemble	
	1. assembleDebug
	2. assembleRelease
```

They both depend on other tasks that execute the multiple steps needed to build an APK. The assemble task depends on both, so calling it will build both APKs.

```
它们都取决于构建APK需要执行多个步骤的其他任务。
该组装任务取决于双方，所以它会同时生成APK。
```

Tip: Gradle support camel case shortcuts for task names on the command line. For instance:

```
提示：Gradle命令行支持camel case快捷方式。
例如：
```

```
gradle aR
```

is the same as typing

```
跟下面这个命令一样：
```

```
gradle assembleRelease
```

as long as no other task match ‘aR’

```
只要没有其他任务匹配到‘aR’
```

The check anchor tasks have their own dependencies:

* check
	* lint
* connectedCheck
	* connectedAndroidTest
	* connectedUiAutomatorTest (not implemented yet)
* deviceCheck
	* This depends on tasks created when other plugins implement test extension points.

```
检查任务有它自己的依赖：

-> check 	－lint
-> connectedCheck	
	－connectedAndroidTest
	－connectedUiAutomatorTest (not implemented yet)
-> deviceCheck	－这取决于当其他插件实现测试扩展点时创建的任务
```
Finally, the plugin creates install/uninstall tasks for all build types (debug, release, test), as long as they can be installed (which requires signing).

```
最后，插件创建所有构建类型(debug, release, test)的安装/卸载任务，只要它们能安装(这需要签字)。
```


##Basic Build Customization [\^](#gradle-plugin-user-guide)

The Android plugin provides a broad DSL to customize most things directly from the build system.

###Manifest entries [\^](#gradle-plugin-user-guide)

Through the DSL it is possible to configure the following manifest entries:

* minSdkVersion
* targetSdkVersion
* versionCode
* versionName
* applicationId (the effective packageName -- see [ApplicationId versus PackageName](http://tools.android.com/tech-docs/new-build-system/applicationid-vs-packagename) for more information)
* Package Name for the test application
* Instrumentation test runner

Example:

```
android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"

    defaultConfig {
        versionCode 12
        versionName "2.0"
        minSdkVersion 16
        targetSdkVersion 16
    }
}
```

The defaultConfig element inside the android element is where all this configuration is defined.

Previous versions of the Android Plugin used packageName to configure the manifest 'packageName' attribute.
Starting in 0.11.0, you should use applicationId in the build.gradle to configure the manifest 'packageName' entry.
This was disambiguated to reduce confusion between the application's packageName (which is its ID) and 
java packages.

The power of describing it in the build file is that it can be dynamic.
For instance, one could be reading the version name from a file somewhere or using some custom logic:

```
def computeVersionName() {
    ...
}

android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"

    defaultConfig {
        versionCode 12
        versionName computeVersionName()
        minSdkVersion 16
        targetSdkVersion 16
    }
}
```
**Note:** Do not use function names that could conflict with existing getters in the given scope. For instance instance defaultConfig { ...} calling getVersionName() will automatically use the getter of defaultConfig.getVersionName() instead of the custom method.

If a property is not set through the DSL, some default value will be used. Here’s a table of how this is processed.

| Property Name | Default value in DSL object	 | Default value
|:------:|:----------:|:----------:
| versionCode |  -1	  | value from manifest if present
| versionName | 	 null	  | value from manifest if present
| minSdkVersion | 	 -1	  | value from manifest if present
| targetSdkVersion | 	 -1	  | value from manifest if present
| applicationId | 	 null	  | value from manifest if present
| testApplicationId | 	 null	  | applicationId + “.test”
| testInstrumentationRunner | 	 null  | android.test.InstrumentationTestRunner
|  signingConfig | 	 null	  | null
|  proguardFile | 	 N/A (set only)	  | N/A (set only)
|  proguardFiles	 |  N/A (set only)	  | N/A (set only)

The value of the 2nd column is important if you use custom logic in the build script that queries these properties. For instance, you could write:

```
if (android.defaultConfig.testInstrumentationRunner == null) {
    // assign a better default...
}
```

If the value remains null, then it is replaced at build time by the actual default from column 3, but the DSL element does not contain this default value so you can't query against it.
This is to prevent parsing the manifest of the application unless it’s really needed. 


###Build Types [\^](#gradle-plugin-user-guide)

By default, the Android plugin automatically sets up the project to build both a debug and a release version of the application.

These differ mostly around the ability to debug the application on a secure (non dev) devices, and how the APK is signed.

The debug version is signed with a key/certificate that is created automatically with a known name/password (to prevent required prompt during the build). The release is not signed during the build, this needs to happen after.

This configuration is done through an object called a BuildType. By default, 2 instances are created, a debug and a release one.

The Android plugin allows customizing those two instances as well as creating other Build Types. This is done with the buildTypes DSL container:

```
android {
    buildTypes {
        debug {
            applicationIdSuffix ".debug"
        }

        jnidebug.initWith(buildTypes.debug)
        jnidebug {
            packageNameSuffix ".jnidebug"
            jniDebuggable true
        }
    }
}
```
The above snippet achieves the following:

* Configures the default debug Build Type:
	* set its package to be <app appliationId>.debug to be able to install both debug and release apk on the same device
* Creates a new BuildType called jnidebug and configure it to be a copy of the debug build type.
* Keep configuring the jnidebug, by enabling debug build of the JNI component, and add a different package suffix.
Creating new Build Types is as easy as using a new element under the buildTypes container, either to call initWith() or to configure it with a closure.

The possible properties and their default values are:

| Property Name | Default value for debug	 | Default value for release / other
|:------:|:----------:|:----------:
| debuggable	| true	| false
| jniDebuggable	 | false	| false
| renderscriptDebuggable	| false	| false
| renderscriptOptimLevel	| 3	| 3
| applicationIdSuffix	 | null	| null
| versionNameSuffix	| null	| null
| signingConfig	| android.signingConfigs.debug	| null
| zipAlignEnabled	| false	| true
| minifyEnabled	 | false	| false
| proguardFile	| N/A (set only)	| N/A (set only)
| proguardFiles	| N/A (set only)	| N/A (set only)
 
In addition to these properties, Build Types can contribute to the build with code and resources.

For each Build Type, a new matching sourceSet is created, with a default location of

```
src/<buildtypename>/
```

This means the Build Type names cannot be main or androidTest (this is enforced by the plugin), and that they have to be unique to each other.

Like any other source sets, the location of the build type source set can be relocated:

```
android {
    sourceSets.jnidebug.setRoot('foo/jnidebug')
}
```

Additionally, for each Build Type, a new assemble<BuildTypeName> task is created.

The assembleDebug and assembleRelease tasks have already been mentioned, and this is where they come from. When the debug and release Build Types are pre-created, their tasks are automatically created as well.

The build.gradle snippet above would then also generate an assembleJnidebug task, and assemble would be made to depend on it the same way it depends on the assembleDebug and assembleRelease tasks.

Tip: remember that you can type gradle aJ to run the assembleJnidebug task.

Possible use case:

* Permissions in debug mode only, but not in release mode
* Custom implementation for debugging
* Different resources for debug mode (for instance when a resource value is tied to the signing certificate).

The code/resources of the BuildType are used in the following way:

* The manifest is merged into the app manifest
* The code acts as just another source folder
* The resources are overlayed over the main resources, replacing existing values.

###Signing Configurations [\^](#gradle-plugin-user-guide)

Signing an application requires the following:

* A keystore
* A keystore password
* A key alias name
* A key password
* The store type

The location, as well as the key name, both passwords and store type form together a Signing Configuration (type SigningConfig)

By default, there is a debug configuration that is setup to use a debug keystore, with a known password and a default key with a known password.
The debug keystore is located in $HOME/.android/debug.keystore, and is created if not present.

The debug Build Type is set to use this debug SigningConfig automatically.

It is possible to create other configurations or customize the default built-in one. This is done through the signingConfigs DSL container:

```
android {
    signingConfigs {
        debug {
            storeFile file("debug.keystore")
        }

        myConfig {
            storeFile file("other.keystore")
            storePassword "android"
            keyAlias "androiddebugkey"
            keyPassword "android"
        }
    }

    buildTypes {
        foo {
            debuggable true
            jniDebuggable true
            signingConfig signingConfigs.myConfig
        }
    }
}
```

The above snippet changes the location of the debug keystore to be at the root of the project. This automatically impacts any Build Types that are set to using it, in this case the debug Build Type.

It also creates a new Signing Config and a new Build Type that uses the new configuration.

**Note:** Only debug keystores located in the default location will be automatically created. Changing the location of the debug keystore will not create it on-demand. Creating a SigningConfig with a different name that uses the default debug keystore location will create it automatically. In other words, it’s tied to the location of the keystore, not the name of the configuration.

**Note:** Location of keystores are usually relative to the root of the project, but could be absolute paths, thought it is not recommended (except for the debug one since it is automatically created).

**Note:** If you are checking these files into version control, you may not want the password in the file. The following Stack Overflow post shows ways to read the values from the console, or from environment variables.
[how to create release signed apk](http://stackoverflow.com/questions/18328730/how-to-create-a-release-signed-apk-file-using-gradle)
We'll update this guide with more detailed information later.

###Running ProGuard [\^](#gradle-plugin-user-guide)

ProGuard is supported through the Gradle plugin for ProGuard version 4.10. The ProGuard plugin is applied automatically, and the tasks are created automatically if the Build Type is configured to run ProGuard through the minifyEnabled property.

```
android {
    buildTypes {
        release {
            minifyEnabled true
            proguardFile getDefaultProguardFile('proguard-android.txt')
        }
    }

    productFlavors {
        flavor1 {
        }
        flavor2 {
            proguardFile 'some-other-rules.txt'
        }
    }
}
```

Variants use all the rules files declared in their build type, and product flavors.

There are 2 default rules files

* proguard-android.txt
* proguard-android-optimize.txt

They are located in the SDK. Using getDefaultProguardFile() will return the full path to the files. They are identical except for enabling optimizations.

#Dependencies, Android Libraries and Multi-project setup [\^](#gradle-plugin-user-guide)

Gradle projects can have dependencies on other components. These components can be external binary packages, or other Gradle projects.

##Dependencies on binary packages [\^](#gradle-plugin-user-guide)

###Local packages [\^](#gradle-plugin-user-guide)

To configure a dependency on an external library jar, you need to add a dependency on the compile configuration.

```
dependencies {
    compile files('libs/foo.jar')
}

android {
    ...
}
```

**Note:** the dependencies DSL element is part of the standard Gradle API and does not belong inside the android element.

The compile configuration is used to compile the main application. Everything in it is added to the compilation classpath and also packaged in the final APK.

There are other possible configurations to add dependencies to:

* compile: main application
* androidTestCompile: test application
* debugCompile: debug Build Type
* releaseCompile: release Build Type.

Because it’s not possible to build an APK that does not have an associated Build Type, the APK is always configured with two (or more) configurations: compile and <buildtype>Compile.

Creating a new Build Type automatically creates a new configuration based on its name.

This can be useful if the debug version needs to use a custom library (to report crashes for instance), while the release doesn’t, or if they rely on different versions of the same library.

###Remote artifacts [\^](#gradle-plugin-user-guide)

Gradle supports pulling artifacts from Maven and Ivy repositories.

First the repository must be added to the list, and then the dependency must be declared in a way that Maven or Ivy declare their artifacts.

```
repositories {
    mavenCentral()
}


dependencies {
    compile 'com.google.guava:guava:11.0.2'
}

android {
    ...
}
```

**Note:** mavenCentral() is a shortcut to specifying the URL of the repository. Gradle supports both remote and local repositories.

**Note:** Gradle will follow all dependencies transitively. This means that if a dependency has dependencies of its own, those are pulled in as well.

For more information about setting up dependencies, read the Gradle user guide [here](http://gradle.org/docs/current/userguide/artifact_dependencies_tutorial.html), and DSL documentation [here](http://gradle.org/docs/current/dsl/org.gradle.api.artifacts.dsl.DependencyHandler.html).

#Testing [\^](#gradle-plugin-user-guide)

#Build Variants [\^](#gradle-plugin-user-guide)

#Advanced Build Customization [\^](#gradle-plugin-user-guide)