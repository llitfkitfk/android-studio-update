###Android Studio 0.1.3 Released

####Notable Changes:

* The source editors, and the layout preview and layout editor views, are now "Gradle project aware", meaning that resources defined in flavors and build types are now properly handled and rendered. When you switch variants, the resources should be recomputed.

![](https://github.com/llitfkitfk/android-studio-update/blob/master/as-images/variants.png)

* There were a lot of fixes in the Gradle project import and build areas. There is now a "sync" button in the toolbar which will reimport the Gradle project state into your Android Studio project. Use this after editing your Gradle files, for example to add a library. In the future we will more automatically handle state syncing, but for now this is the simplest way to keep the IDE up to date with project structure changes made to the Gradle files.

![](https://github.com/llitfkitfk/android-studio-update/blob/master/as-images/reimport.png)

* One bug fix should make module dependency handling in Gradle projects work better, so you will hopefully no longer need to manually work around missing dependencies with the project structure dialog. As with previous builds, you still need to edit your Gradle files to actually add new libraries.

* We've switched from an older snapshot of IntelliJ to a recent one. This should pick up recent improvements in the core IDE itself.

* In the editor, you can now invoke resource info (View > Quick Documentation) on XML resource references such as @string/foo (in older versions this only worked on R.string.foo references in .java files).

* There is a new lint check, and an Android Studio editor quickfix, to replace hardcoded resource packages with the auto resource package, which is vital in Gradle projects where the application package can vary between the build outputs.

![](https://github.com/llitfkitfk/android-studio-update/blob/master/as-images/resauto.png)

* Finally, there are lots of bug fixes, primarily to address crash reports submitted by preview users (Thank you for submitting these!).

####Installation Notes
If you are already running Android Studio, just restart it, or manually check for updates via Help > Check for Update... (on OSX, look in the Android Studio menu). This will download and install a small patch rather than download a full IDE image. We are not posting full installers for each weekly update. If you do not already have Android Studio, install the latest full install from [here](http://developer.android.com/sdk/installing/studio.html#download)

When you run that version, it will check for updates and upgrade itself via the patch mechanism.

![](https://github.com/llitfkitfk/android-studio-update/blob/master/as-images/update-available.png)
