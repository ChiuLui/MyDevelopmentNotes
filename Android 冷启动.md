Android 冷启动
-------------

# 简介:
> 本篇文章主要简单介绍 Android 冷启动的几种实现方式。

# 目录:
[1.什么是冷启动](#1)

[2.冷启动的实现方式](#2)



# <span id = "1">**1.什么是冷启动**</span>

### 冷启动介绍:


- 当手机 重启 ，点击桌面图标启动应用的过程就是冷启动
- 未启动手机，长时 未使用，应用被 kill 后，此时点击桌面图标启动应用的过程

![冷启动图例](https://upload-images.jianshu.io/upload_images/1760510-511c02a72c475c0c.gif?imageMogr2/auto-orient/strip|imageView2/2/w/470)

### 冷启动的表现形式：

##### 未做处理的情况

- 点击桌面图标后没有反应，没有瞬间打开应用，也就是没有马上看到应用打开
- 点击桌面图标后会显示 黑屏 或者 白屏 ， 没有及时渲染出页面元素



# <span id = "2">**2.冷启动的实现方式**</span>

### 从根本上解决：

&emsp;&emsp;冷启动无法避免，我们只能去减少冷启动的时间和适配冷启动。

- 减少在 Application 中的耗时操作(懒加载)
- 减少在 onCreate 的耗时操作


### 实现方式一：

##### 纯色背景 + 启动图标

&ensp;&ensp;这种做法在国产APP上面少见，在国外的APP常见，简单的来说就是用 layer-list 绘制一个纯色的背景加上一个启动图标，layer-list 代码如下：


```

<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:drawable="@color/colorPrimary" />

    <item>
        <bitmap
            android:gravity="center"
            android:src="@mipmap/ic_launcher" />
    </item>

</layer-list>


```


`然后我们为SplashActivity创建一个主题：`

```

<resources>
    <!-- 基本主题 -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <!--纯色加启动图标的方案-->
    <style name="SplashThemeLayer" parent="AppTheme">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <item name="android:windowBackground">@drawable/bg_splash_layer_list</item>
    </style>
</resources>



```



![](https://upload-images.jianshu.io/upload_images/1760510-6adc32c300df24e2.gif?imageMogr2/auto-orient/strip|imageView2/2/w/470)


### 实现方式二：

##### 纯使用背景图片

&ensp;&ensp;前面的第一种方式是使用纯色背景 + 启动图标，这种方式肯定是不满足我们的产品经理的，他们要的是 个性化 的页面。

> 使用背景图片也是很简单的，只需要在them将我们之前的drawable替换成我们的图片即可：


```

<!--使用图片的方案-->
    <style name="SplashThemeImage" parent="AppTheme">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <item name="android:windowBackground">@mipmap/icon_splish</item>
        <!--沉浸-->
        <item name="android:windowTranslucentStatus">true</item>
    </style>



```


> 需要注意的是：Splash页面的背景颜色需要设置为透明 #00000000，不要设置其他背景，否则会导致图片的伸缩变形。


![](https://upload-images.jianshu.io/upload_images/1760510-99749cd115a60e6e.gif?imageMogr2/auto-orient/strip|imageView2/2/w/470)


