---
title: DataBinderMapperImpl无法生成
date: 2018-12-26 20:10:24
categories:
- 技术
- android
- 问题解决
tags:
- databinding
- 技术
- android
---

MVC的一个老项目，需要从另一个MVVM的项目里移植过来一些功能，新建了一个module开启了databinding后报错

```
ClassNotFoundException: Didn't find class "android.databinding.DataBinderMapperImpl" on path
```

谷歌之，在Stack Overflow上看到一段提示：

<em> Got my own lib and multiple apps. Had to add both to lib and app gradle files. </em>

<!-- more -->
#### 解决方法

lib module与 app module都需要在各自的 `gradle` 文件中开启databinding：

```java
android {
    ....
    dataBinding {
        enabled = true
    }
}
```

> 参考：[Stack Overflow](https://stackoverflow.com/questions/40882253/classnotfoundexception-didnt-find-class-android-databinding-databindermapper/42067249)

