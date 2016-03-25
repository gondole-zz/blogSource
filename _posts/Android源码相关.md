title: Android源码相关
date: 2016-03-24 13:55:32
tags: [Android, 源码]
categories: Android
---
![](http://7xqdqt.com1.z0.glb.clouddn.com/2016%2F03%2F24%2Fwallhaven-329431.jpg)

## SDK自带源码相关

在开发工具里都可以关联到SDK自带的Source源码，Eclipse和Android Studio方式大概相同，大部分的API都可以找到，少部分的API可能出于安全原因，在Source源码里并不能找到，可以到AOSP里去寻找更详细的源码，这些隐藏的API方法也可以通过反射的方式来进行调用。

## AOSP

[AOSP](https://android.googlesource.com/) 包含了Google开源的很多源码，过于庞大，对于一般开发者来说，我们只需要接触Framework层次的东西就够了，这里包括了base、build-tools、support包甚至Volley项目的源码。

<!--more-->

## 开始阅读

1. Handler-Message-Looper
2. Activity和Service
3. Fragment
4. View
5. MotionEvent
6. LayoutInflator
7. SurfaceView和TextureView
8. AsyncTask
9. Volley
10. android.util.*

## 进阶阅读

1. Context
2. ClassLoader
3. Binder
4. WMS，AMS，PMS，NMS，IMS等系统Service

## 开源项目

1. EventBus、OTTO （事件总线）
2. Volley (网络请求)
3. RxJava （异步）
4. Guava （核心基础类库）

## 一些大神和资源

1. [AOSP官方的介绍](https://source.android.com/source/index.html)
2. [官方教程](https://developer.android.com/training/index.html)、[官方博客](http://android-developers.blogspot.sg/)
3. [老罗的Android之旅](http://blog.csdn.net/luoshengyang)
4. [Innost的专栏](http://blog.csdn.net/innost)
