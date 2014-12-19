###Android Studio 1.0.2 Released

We've just released Android Studio 1.0.2 to the canary and dev channels; it should roll out to beta and stable within a day or so. This is just a bug fix release which the following fixes:

* [80594](https://code.google.com/p/android/issues/detail?id=80594): SDK Manager on Windows fails to install tools
* [82998](https://code.google.com/p/android/issues/detail?id=82998): Update recommended SDK Tools version in Android Studio from 24.0.1
* [79778](https://code.google.com/p/android/issues/detail?id=79778): Package manager exception when using custom signing
* [82999](https://code.google.com/p/android/issues/detail?id=82999): NPE in com.intellij.ide.navigationToolbar.NavBarPanel
* [82702](https://code.google.com/p/android/issues/detail?id=82702): Objectify filter added to web.xml when generating endpoints from class, even when the class is not an objectify entity


```
我们刚刚在canary和dev频道发布了Android Studio 1.0.2版本；过后会被放倒beta和stable频道。
这个版本仅仅修复了一些bug：(bug描述详见原文链接)

-> 解决了Windows版本不能安装工具的问题
-> 更新推荐的SDK工具版本到24.0.1
-> 解决了使用自定义签名打包出现的管理exception问题
-> 解决了 com.intellij.ide.navigationToolbar.NavBarPanel 出现的NullPointerException的问题
-> 解决了当生成endpints，web.xml的runtime问题
```