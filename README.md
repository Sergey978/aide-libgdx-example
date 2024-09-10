# aide-libgdx-example
Do you want ability to work in modern Android Studio and continue working from mobile device? Here it is. Using gradle in Android Studio and eclipse ant project files in AIDE

Note: this approach is obsolete. Modern AIDE support regular gradle structured projects. See  https://github.com/Deepscorn/libgdx-gradle-template

# How it works
You have regular gradle LibGdx project in Android Studio / IDEA on desktop. You run gradle task to generate eclipse ant project files (.project + .classpath). They are used instead of build.gradle in AIDE.

It's an example to article http://deepscorn.blogspot.ru/2016/07/building-and-running-android-studio.html

---------------Article----------

Building and running Android Studio gradle project on android device
Recently I was just checking out if AIDE team added support to build gradle-based projects. So, indeed they added:

AIDE also supports basic Android Studio projects, which follow the default project structure. The full gradle build system is not yet supported though
(source: http://www.android-ide.com/tutorial_androidstudio.html)

So, if your gradle setup is simple, you can just open your android project in AIDE and run.


But what to do if not? As before we have a workarond: create eclipse project files (.classpath + .project) for our project. AIDE works with eclipse project structure quite well. But what to do with the dependencies? It's insane to write all of them by hand. Likely, LibGdx enthusiasts gives us script which generates those strings for us (see for example eclipse task here)

I'll give details on example of how to build & run LibGdx gradle project in AIDE.


1. Generate new LibGdx gradle project. I recommend using https://github.com/czyzby/gdx-setup for that because it has more cool customiztion options and is better maintained than official gdx-setup

2. Open Android Studio and build project. This is needed to make sure all the project dependencies are cached in standard gradle directory on your computer (C:/users/<username>/.gradle/caches/modules-2/files-2.1/ for Windows).

3. Copy contents of that directory somewhere on your mobile device, for example to:

<AIDE AppProjects directory>/.gradle/caches/modules-2/files-2.1/

4. Run (from root of your project)
./gradlew -t eclipse to generate eclipse project files

5. Edit core/.classpath and android/.classpath:

For each dependency we need to replace paths so that they refer to gradle cache which we copied to mobile device earlier.

Example:
<classpathentry kind="lib" path="C:/users/iz/.gradle/caches/modules-2/files-2.1/com.badlogicgames.gdx/gdx-backend-android/1.9.3/c45573bf68b55442990936f007e7d6823d259c1f/gdx-backend-android-1.9.3.jar"/>

Will become:

<classpathentry kind="lib" path="/storage/emulated/0/AppProjects/.gradle/caches/modules-2/files-2.1/com.badlogicgames.gdx/gdx-backend-android/1.9.3/c45573bf68b55442990936f007e7d6823d259c1f/gdx-backend-android-1.9.3.jar"/>

7. Place all the project assets in android/scr/main/assets. This makes AIDE automatically include them in apk

8. You also need to copy by hand all *.so from android/libs to android/libs on mobile device. You need to do that each time libraries gets updated or runtime crash can occur

9. One final step is to remove build.gradle files. It's to make sure AIDE will use eclipse project files and not the gradle ones

Now navigate to android directory in AIDE, open Android project, click run and here you go!

Source code of working example: https://github.com/Deepscorn/aide-libgdx-example
