---
layout: post
title: 使用AndroidStudio进行NDK开发，helloNDK
categories: [JAVA,Android]
tags: [Android]
comments: true
description: NDK开发网上已经有许多教程，但是由于工具的版本和开发环境的不一样，我在开发HelloNDK时还是碰到不少问题，包括一些坑爹问题。这里记录下我的HelloNDK开发。
---
先介绍下我的开发环境。mac OS ，androidStudio1.5.1.

下面是官方NDK教程，英文版。

https://codelabs.developers.google.com/codelabs/android-studio-jni/index.html#0

http://tools.android.com/tech-docs/android-ndk-preview

##创建HelloNDK项目

##设置gradle插件

打开`build.gradle(Project:HelloNDK)`文件，替换

`classpath 'com.android.tools.build:gradle:1.5.0'`

为

`classpath 'com.android.tools.build:gradle-experimental:0.6.0-alpha5'`

打开`gradle-wrapper.properties`文件，替换

`distributionUrl=https\://services.gradle.org/distributions/gradle-2.8-all.zip`

为

`distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip`

<i color=red>注意：gradle-experimental与gradle版本是有依赖关系的，其关系请参考上面给出的官方文档。</i>

打开`build.gradle(Module:app)`文件，参考下面的内容进行替换，


<pre>
apply plugin: "com.android.model.application"

model {
    android {
        compileSdkVersion 23
        buildToolsVersion "23.0.2"

        defaultConfig {
            applicationId "com.lengyue.hellondk"
            minSdkVersion.apiLevel 15
            targetSdkVersion.apiLevel 23
            versionCode 1
            versionName "1.0"

        }
        buildTypes {
         release {
             minifyEnabled false
             proguardFiles.add(file("proguard-rules.pro"))
         }
     }
     }
}
</pre>

更多配置可以参考http://tools.android.com/tech-docs/new-build-system/gradle-experimental

打开Module Setting ->SDK Location设置NDK路径。

到这里环境配置完成。

##NDK开发

设置native库名，打开`build.gradle(Module:app)`文件

{% highlight Bash %}
buildTypes {
...
}
// New code
ndk {
    moduleName = "helloNDK"
}
// New code finished
{% endhighlight %}

Layout中添加一个TextView然后MainActivity中设置TextView的值和添加native方法

{% highlight java %}
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        TextView tv_hello = (TextView) findViewById(R.id.tv_hello);
        tv_hello.setText(getStringFromNative());
    }

    public native String getStringFromNative();

    static {
        System.loadLibrary("helloNDK");
    }
}
{% endhighlight %}

鼠标放在native方法上使用快捷键alt+enter唤出快捷菜单，选择生成JNI方法。
在app/src/main目录下会生成jni目录，并且生成helloNDK.c文件。我们注意到c文件名与上面设置的moduleName是一致的。

我在这里出现了错误，但是并不影响,应该是IDE插件的问题。

`Element: class com.intellij.psi.impl.source.tree.PsiErrorElementImpl because: containing file is null
           invalidated at: see attachment`

编辑helloNDK.c文件

{% highlight c %}
#include <jni.h>

JNIEXPORT jstring JNICALL
Java_com_lengyue_hellondk_MainActivity_getStringFromNative(JNIEnv *env, jobject instance)
{
    return (*env)->NewStringUTF(env, "Hello NDK");
}
{% endhighlight %}

到这里HelloNDK开发完成。点击run即可在真机上测试。

#后记：

网上的教程好多都写到要使用javah命令来生成.h头文件。在这里我碰到了问题。下面是我成功生成.h文件的终端命令

`cd src/main`

`javah -d jni -classpath ~/developEnv/android-sdk/sdk/platforms/android-23/android.jar:/Users/lihang/developEnv/android-sdk/sdk/extras/android/m2repository/com/android/support/appcompat-v7/23.1.1/appcompat-v7-23.1.1-sources.jar:../.build/intermediates/classes/debug com.lengyue.hellojni.MainActivity`

网上教程javah命令中-classpath值都是使用“;”分隔的，然而我这里是使用“:”分隔的。不知道是windows和mac上的jdk差异还是什么问题，这里记录下。

按照网上的编辑mk文件然后执行ndk-build命令，我发现生成的apk中动态链接库so文件名为libapp.so而不是我们mk文件中设置的库文件名和ndk-build生成的对应mk文件的so文件。

应该是androidStudio的ndk插件已经不支持原来的方式开放了吧？看教程使用的androidStudio版本还是0.4版本。
