---
title: 关于Android Studio3.0升级
date: 2017-10-27 14:45:16
categories: 
- 技术
- android
- android studio
tags:
- 技术
- android
- android studio
---

#### 升级3.0之后打开项目报错，这里记录一下解决的过程 

首先更新了gradle版本

<img src="http://oph0392na.bkt.clouddn.com/2017-10-27-屏幕快照 2017-10-27 15.48.27.png" height="1269" width="585">

<!-- more -->

之后报错
``` 
Gradle sync failed: Cannot set the value of read-only property 'outputFile' for ApkVariantOutputImpl_Decorated{apkData=Main{type=MAIN, fullName=xiaomiDebug, filters=[]}} of type com.android.build.gradle.internal.api.ApkVariantOutputImpl.
```
Google了一下，是因为项目Gradle下使用的部分代码语法变了
这是项目中原本自定义构建APK的代码
``` gradle
 applicationVariants.all { variant ->
        variant.outputs.each { output ->
            def newName
            def timeNow
            def outputFile = output.outputFile
            if (Boolean.valueOf(IS_JENKINS)) {
                timeNow = JENKINS_TIME
                newName = APP_PACKAGE_NAME + "-android-V${defaultConfig.versionName}-" + variant.productFlavors[0].name + "-" + timeNow + "-" + variant.buildType.name + '.apk'
            } else {
                timeNow = getDate()
                if ('debug' == variant.buildType.name) {
                    newName = APP_PACKAGE_NAME + "-android-V${defaultConfig.versionName}-debug.apk"
                } else {
                    newName = APP_PACKAGE_NAME + "-android-V${defaultConfig.versionName}-" + variant.productFlavors[0].name + "-" + timeNow + "-" + variant.buildType.name + '.apk'
                }
            }
            output.outputFile = new File(outDirectory, newName)
        }
    }
```
其中的这句 `variant.outputs.each { output ->`
需要把`each`替换成`all`
获取输出文件的代码`output.outputFile`已经变为只读,现在直接使用`outputFileName`赋值即可修改输出的名称

修改后的代码
``` gradle
applicationVariants.all { variant ->
        variant.outputs.all { output ->
            def newName
            def timeNow
            if (Boolean.valueOf(IS_JENKINS)) {
                timeNow = JENKINS_TIME
                newName = APP_PACKAGE_NAME + "-android-V${defaultConfig.versionName}-" + variant.productFlavors[0].name + "-" + timeNow + "-" + variant.buildType.name + '.apk'
            } else {
                timeNow = getDate()
                if ('debug' == variant.buildType.name) {
                    newName = APP_PACKAGE_NAME + "-android-V${defaultConfig.versionName}-debug.apk"
                } else {
                    newName = APP_PACKAGE_NAME + "-android-V${defaultConfig.versionName}-" + variant.productFlavors[0].name + "-" + timeNow + "-" + variant.buildType.name + '.apk'
                }
            }
            outputFileName = newName
        }
    }
```
修改后 Try Again 继续报错

```
Gradle sync failed: Cannot choose between the following configurations of project :androidSDK:
			- debugApiElements
			- debugRuntimeElements
			- releaseApiElements
			- releaseRuntimeElements
			All of them match the consumer attributes:
			- Configuration 'debugApiElements':
			- Found com.android.build.api.attributes.BuildTypeAttr 'debug' but wasn't required.
			- Found com.android.build.gradle.internal.dependency.AndroidTypeAttr 'Aar' but wasn't required.
			- Found com.android.build.gradle.internal.dependency.VariantAttr 'debug' but wasn't required.
			- Found org.gradle.api.attributes.Usage 'java-api' but wasn't required.
			- Configuration 'debugRuntimeElements':
			- Found com.android.build.api.attributes.BuildTypeAttr 'debug' but wasn't required.
			- Found com.android.build.gradle.internal.dependency.AndroidTypeAttr 'Aar' but wasn't required.
			- Found com.android.build.gradle.internal.dependency.VariantAttr 'debug' but wasn't required.
			- Found org.gradle... (show balloon)
```
看报错说明是因为依赖的一个model`:androidSDK`,Google说明新版中依赖项目的语法改变了,依赖包使用旧的语法没有报错,
旧版`compile project(':androidSDK')`
新版`implementation project(':androidSDK')`
貌似现在依赖包也可以使用`implementation`

这里还提示一点,gradle文件中:

`compile` 替换为 `implementation`
 `apt` 替换为 `annotationProcessor` 
 `testCompile` 替换为 `androidTestImplementation`
`releaseCompile` 替换为 `releaseImplementation`

这里注意一点:`compile fileTree(dir: 'libs', include: ['*.jar'])`
不需要替换`compile`,看样子只有依赖包的时候才需要替换
还有一点,有的依赖包替换成`implementation`提示找不到,换回`compile`又可以了


继续Tran Again提示需要更新Android SDK Build Tools version 至 26.0.2
之后报错
```
Error:android-apt plugin is incompatible with the Android Gradle plugin.  Please use 'annotationProcessor' configuration instead.
Error:All flavors must now belong to a named flavor dimension. Learn more at https://d.android.com/r/tools/flavorDimensions-missing-error-message.html
```
继续Google,新版去掉了android-apt,所以删除项目Gradle中`apply plugin: 'android-apt'`,只保留`apply plugin: 'com.android.application'`
把`apt`替换为`annotationProcessor`

Try Again:
```
Error:All flavors must now belong to a named flavor dimension. Learn more at https://d.android.com/r/tools/flavorDimensions-missing-error-message.html
```
根据官网说法：
```
you need to first declare one or more dimensions using the flavorDimensions property. After that, assign each flavor to one of the dimensions you declared
```
必须指定一个或多个 flavor dimensions
flavorDimensions 后面可以添加多个不同类型的参数例如：`flavorDimensions "tier", "minApi"`
官方示例:
``` gradle
// Specifies two flavor dimensions.
flavorDimensions "tier", "minApi"

productFlavors {
     free {
      // Assigns this product flavor to the "tier" flavor dimension. Specifying
      // this property is optional if you are using only one dimension.
      dimension "tier"
      ...
    }

    paid {
      dimension "tier"
      ...
    }

    minApi23 {
        dimension "minApi"
        ...
    }

    minApi18 {
        dimension "minApi"
        ...
    }
}
```
项目中原来的代码:
``` gradle
productFlavors {
        _360 {}
        xiaomi {}
    }
```
修改后的代码:
``` gradle
flavorDimensions "default"
productFlavors {
   _360 {
       dimension "default"
   }
   xiaomi {
       dimension "default"
   }
}
```
继续Tran Again:
```
Error:found unexpected optical bounds (red pixel) on top border at x=102.
Error:java.util.concurrent.ExecutionException: com.android.tools.aapt2.Aapt2Exception: AAPT2 error: check logs for details
Error:Execution failed for task ':ygj_new:merge_360DebugResources'.
> Error: java.util.concurrent.ExecutionException: com.android.tools.aapt2.Aapt2Exception: AAPT2 error: check logs for details
```
是因为项目.9图片中含有红色边线,删除即可

继续Try Again:
```
FAILURE: Build failed with an exception.

* What went wrong:
Task 'compileDebug' is ambiguous in root project 'eachonline_upgrade'. Candidates are: 'compileDebugAidl', 'compileDebugAndroidTestAidl', 'compileDebugAndroidTestJavaWithJavac', 'compileDebugAndroidTestKotlin', 'compileDebugAndroidTestNdk', 'compileDebugAndroidTestRenderscript', 'compileDebugAndroidTestShaders', 'compileDebugAndroidTestSources', 'compileDebugJavaWithJavac', 'compileDebugKotlin', 'compileDebugNdk', 'compileDebugRenderscript', 'compileDebugShaders', 'compileDebugSources', 'compileDebugUnitTestJavaWithJavac', 'compileDebugUnitTestKotlin', 'compileDebugUnitTestSources'.
```

继续Tran Again:
```
Error:Execution failed for task ':ygj_new:transformClassesWithAndroidGradleClassShrinkerFor_360Debug'.
> java.lang.ArrayIndexOutOfBoundsException (no error message)
```
对于这个错误真是百思不得其解,研究了两天在一篇博文的启示下把Instant Run关掉,之后就成功了!
这里附上这篇博文: [记录使用Instant Run的一个坑](http://www.jianshu.com/p/64c819e0d8a3)


