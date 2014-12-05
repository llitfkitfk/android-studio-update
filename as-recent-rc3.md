###Android Studio 1.0 Release Candidate 3 Released

We've just released Android Studio 1.0 Release Candidate 3 to the canary and beta channels. This fixes a number of bugs reported against RC 2 and updates to the latest Gradle plugin.

我们刚刚发布的Android studio 1.0 rc3到canary渠道和beta渠道。修正了一些RC2的bugs以及更新到最新的gradle插件。

One frequent problem users have reported is that editing idea.properties, idea.vmoptions or Info.plist for various reason (e.g. to tweak the VM heap size, or to switch to a different version of the JDK, etc) has resulted in patch update warnings.


用户报告一个常见的问题是 由于各种原因需要编辑idea.properties，idea.vmoptions或Info.plist（例如调整VM heap大小，或者切换到不同版本的JDK等）会导致补丁更新警告。

As of RC 3, we have a better mechanism for customizing properties for the launchers on all three platforms. You should not edit any files in the IDE installation directory. Instead, you can customize the attributes by creating your own .properties or .vmoptions files in the following directories. (This has been possible on some platforms before, but it required you to copy and change the entire contents of the files. With the latest changes these properties are now additive instead such that you can set just the attributes you care about, and the rest will use the defaults from the IDE installation).

因此RC3，我们做了一个更好的机制来自定义所有三个平台（windows，mac， linux）上的配置文件。用户不用再编辑android studio IDE安装目录里的任何文件。相反，用户可以通过下面的目录来创建自己的.properties或.vmoptions文件。 （之前在某些平台上就可以修改并实现，但需要用户复制和更改文件夹的全部内容。凭借最新的变更，用户只要设定用户自己所关心的属性，其余将使用默认配置）


Windows:

```
%USERPROFILE%\.AndroidStudio\studio[64].exe.vmoptions
%USERPROFILE%\.AndroidStudio\idea.properties
```
Mac:

```
~/Library/Preferences/AndroidStudio/studio.vmoptions
~/Library/Preferences/AndroidStudio/idea.properties
```
Linux:

```
~/.AndroidStudio/studio[64].vmoptions
~/.AndroidStudio/idea.properties
```

You can also place use environment variables to point to specific override files elsewhere:

用户也可以配置环境变量指向特定的文件:

```
STUDIO_VM_OPTIONS, which vmoptions file to use
STUDIO_PROPERTIES, which property file to use
STUDIO_JDK, which JDK to run studio with
```

Note that this last variable allows you to for example run Android Studio with Java 7 on OSX (which normally picks Java 6 from the version specified in Info.plist):

注意：最新的变更允许用户运行android studio 在 OS X java 7的环境下(Info.plist的某些版本通常会使用java 6 )

```
$ export STUDIO_JDK=/Library/Java/JavaVirtualMachines/jdk1.7.0_67.jdk
$ open /Applications/Android\ Studio.app
```