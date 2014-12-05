###Android Studio 1.0 Release Candidate 4 Released

We've just released Android Studio 1.0 Release Candidate 4 to both the canary and beta channels.

```
我们刚刚发布的Android studio 1.0 rc4 到canary渠道和beta渠道。
```

This build is almost identical to RC 3 posted a few hours ago, with just a couple of tweaks:

* It bundles Android Gradle plugin 1.0.0-rc4 instead of rc3, which fixes a couple of important Gradle plugin issues
* It includes the Import Sample wizard which was accidentally left out in earlier RCs
* It updates the project structure dialog to use the new property names of a few recently renamed Gradle DSL methods
* Launching an emulator from the AVD window now closes the AVD list window in order to immediately show emulator output

```
此版本跟几小时前更新的RC3几乎是相同的，只是一些细微的调整：

-> 绑定了Gradle插件 1.0.0-RC4来代替RC3，修复了几个重要的gradle插件问题
-> 包含了之前不小心漏掉的功能: 导入向导示例
-> 更新了项目结构对话框：使用一些最近重命名的Gradle DSL方法的属性名
-> 为了立即显示模拟器, 从AVD窗口启动模拟器现在可以关闭AVD列表窗口
```

We have posted patches to the beta and canary channels, and you can also download .zip files for the builds from the 1.0 RC 4 page. We won't post updated installers for this build; just run the RC 3 installer and then patch update to RC 4.

```
我们已经发布了beta和canary渠道的补丁，而且你还可以在1.0 RC4页面下载.zip文件。
我们不会公布最新的安装程序;只需运行RC3安装程序，然后用补丁更新到RC4。
```