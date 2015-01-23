###Android Studio 1.1 Preview 1 Released 



我们刚刚在canary和dev频道发布了Android Studio 1.1 预览版本。

1.1版本的重点是bug修复;我们正在并行的1.2版本，我们正在迁移到的IntelliJ 14的代码库，以及其他功能的工作。

然而，除了bug修复也有一些显着的改进：
1.新项目现在创建启动图标作为@mipmap的资源，而不是@drawable （见http://android-developers.blogspot.com/2014/10/getting-your-apps-ready-for-nexus-6-and 。 HTML更多）
2.各种“清理”探测器皮棉（例如检查FragmentTransactions承诺，并且TypedArrays被回收，等等） ，现在在IDE内逐步运行。他们还一直延伸到搜索其他问题，如缺少数据库光标收市话费，或失踪SurfaceTextures发布呼吁：

The focus for version 1.1 is bug fixes; we are working in parallel on version 1.2 where we are migrating to the IntelliJ 14 codebase, among other feature work. 

However, in addition to the bug fixes there are some notable improvements:
1. New projects now create launcher icons as @mipmap resources instead of @drawable (see http://android-developers.blogspot.com/2014/10/getting-your-apps-ready-for-nexus-6-and.html for more)
2. The various "cleanup" detectors for lint (e.g. checking that FragmentTransactions are committed, and that TypedArrays are recycled, and so on), now run incrementally within the IDE. They've also been extended to search for additional problems, such as missing close calls on database cursors, or missing release calls on SurfaceTextures:

![](http://tools.android.com/_/rsrc/1421370620147/recent/androidstudio11preview1released/cleanup.png)
3. There are a couple of new lint checks:
A lint check which tries to identify string resources that should probably be using plurals instead
A lint check which warns you that @android:string/yes returns OK, not Yes
Several other lint checks that were bytecode based and only ran from Gradle (not lint in the IDE) have been ported to run incrementally in the IDE: Uses of SimpleDateFormat which should probably use getDateInstance instead, checks that addJavaScriptInterface points to a class annotated with @JavaScriptInterface, and a check looking for leaked Handler objects.
4. There is a new template for creating watch faces for Android Wear:

![](http://tools.android.com/_/rsrc/1421370620201/recent/androidstudio11preview1released/watchface-template.png)

The following bugs in the issue tracker were fixed:
[82378]: Android Studio doesn't start, unable to find valid JVM
[92858]: Restrict IconDensities check with splits density data
[81597]: Incorrect inspection about Android problem for non-Android project
[80668]: Lint report doesn't explain how to suppress warnings from Gradle.
[80679]: tools:background should not trigger an overdraw warning
[92789]: False positive in lint PropertyEscape
[82588]: Lint: Make TypographyQuotes work with plurals
[82861]: Library project is created with launcher icon resources
[82862]: The xxhdpi launcher icon differs from other densities
[82351]: src, layout folders are empty while creating new project
[78625]: AVD Default orientation
[80940]: Update lint to ECJ 4.4, ASM 5.0.3: NullPointerException by running lint
[80872]: Don't match resource names for format-parameter only strings
[82634](): "Palette" show twice in View > Tools Window
[94499](): Fixing the device preview in the search and create cases
[82564](): Making AVD Manager separate (non-modal) window.
[77158](): Allow settings.gradle to include projects dynamically
[76923]: can't create outgoing link from widgets that are in included layouts
[81908]: Check if mksdcard can be executed
[82837]: Update wizards to use new headers
[82764]: AVD Manager: SD Card Radio Button Selection
[82991]: AVD Manager: Fix Key lines of the Verify Configuration Screen
[82106]: install, bad link to linux KVM
[77889]: save screenshot: respect device art masks
[81525]: Added "Download JDK 7" quick-fix.
[82813]: uncommitted fragment transactions not highlighted by lint
[81396]: @DrawableRes doesn't match R.mipmap drawables
[79629]: Translations Editor does not show newly added locale
[82768]: Making the AVD ID field look non-editable
[93158]: Properly handle parent class lookup in the API check for this-expressions
[78382]: Lint uses incorrect API level while analyzing Java Library modules
[91988]: FlagManager asserts on region code es-419
[74568]: ADV Manager has start button but no stop button!
80494: Move Clear All and Scroll to End actions
82203: Installer now waits for uninstaller
82126: studio.exe can again run on Win64
83198: NPE when missing dependency during import
82770: Updating api distributions and distribution dialog text
82812: Updating title of AVD Manager window
78668: Adding hardware buttons for nexus one and nexus s
82184:  file paths (in local.properties) are no longer treated as relative if they belong to a different platform.
82837: Use black icons only
81166: device chooser: Prefer to use a device rather than the emulator
82852: After switching device types, the system image list broke
82753: Use $ as separator inner classes in fragment names
77635: Fix scaling of device diagram.
82503: Adding defaults for new avd creation wizard
81739: Fixing avd duplication
79105: Fixing button styles in avd manager
81768: Compute default parameter values quickly
81662: Fixing skin chooser on windows
82282: Do not require approval of all licenses
81342: Improve JDK detection algorithm
77953: NPE on configuring library documentation
82159: Closing exported device files.
81713: Try to detect jdk location automatically on entering the step
81346: Visual feedback for 'Detect JDK' button processing
81620: Do not download samples
79778: apk installation: do not throw error for unexpected dumpsys output
81499: device chooser: special case Google APIs target
+ Misc other fixes for crashes reported via the crash reporter
+ Bug fixes not tagged with a bug number in the commit message.
This does not include the work and fixes to the build system for Gradle plugin 1.1, still in progress. Note that Android Studio 1.1 works fine with version 1.0 of the Android Gradle plugin.

**原文链接：[Android Studio 1.1 Preview 1 Released](http://tools.android.com/recent/androidstudio11preview1released)**