---
title: '切换ViewPager,RecyclerView自动滑动问题'
date: 2017-11-09 19:25:32
categories:
- 技术
- android
- 问题解决
tags:
- viewpager
- fragment
- recyclerview
- 滑动
- 问题
---
#### ViewPager+Fragment+RecyclerView,当切换ViewPager时,RecyclerView自动总是自动滑动
这个问题应该是ViewPager在切换时RecyclerView获得了焦点
RecyclerView的 `focusableOnTouchMode`属性默认是`true`，所以ViewPager切换时recyclerView自动获得焦点后就会发生滚动
解决办法是将recyclerView的父控件设置`android:focusableInTouchMode="true"`

<!-- more -->


