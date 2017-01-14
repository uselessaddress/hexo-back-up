title: Android开发——帧动画
toc: true
date: 2015-07-18 23:29:27
tags:
- android
categories: 设计开发
---

# 前言
获得成就感是学习的灵魂！对于编程，尤其如此。模仿着做一些简单的应用先，之后再补充知识点。This is the plan !

# 应用简介
单击手机屏幕，播放动画。

# 设计思路
1、拷贝关键帧（图片）到资源文件夹。
2、在xml文件中定义关键帧、帧顺序、切换时间。
PS：一般切换时间为1/24秒，我们这里用0.1秒。
3、图片显示在ImageView。
4、响应单击事件。
5、使用AnimationDrawable绘制动画。

<!--more-->

# 实现流程

## 新建Project
新建Project，命名为Demo。

## 重命名app
app重命名为frameanimation，或者删除app，新建Module，命名为frameanimation。

## 准备素材
1、百度“帧图片素材”，找到一套自己喜欢的帧图片。
2、图片重命名为以字母开头，这里小编准备的图片名称为fight_01.png到fight_16.png。
3、将图片拷贝到src/main/res/drawable文件夹。
PS：小编尝试在res新建image文件夹，结果不可以使用，不知道为什么。

## 新建xml文件
右击drawable文件夹，New，Drawable resource file，File name随意（小编起名为frameanimation.xml），Root element选择animation-list。
frameanimation.xml内容如下：

```
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="true">
    <item android:drawable="@drawable/fight_01" android:duration="100" />
    <item android:drawable="@drawable/fight_02" android:duration="100" />
    <item android:drawable="@drawable/fight_03" android:duration="100" />
    <item android:drawable="@drawable/fight_04" android:duration="100" />
    <item android:drawable="@drawable/fight_05" android:duration="100" />
    <item android:drawable="@drawable/fight_06" android:duration="100" />
    <item android:drawable="@drawable/fight_07" android:duration="100" />
    <item android:drawable="@drawable/fight_08" android:duration="100" />
    <item android:drawable="@drawable/fight_09" android:duration="100" />
    <item android:drawable="@drawable/fight_10" android:duration="100" />
    <item android:drawable="@drawable/fight_11" android:duration="100" />
    <item android:drawable="@drawable/fight_12" android:duration="100" />
    <item android:drawable="@drawable/fight_13" android:duration="100" />
    <item android:drawable="@drawable/fight_14" android:duration="100" />
    <item android:drawable="@drawable/fight_15" android:duration="100" />
    <item android:drawable="@drawable/fight_16" android:duration="100" />
</animation-list>

```

一起来分析一下这个文件：

- android:oneshot="true"，默认循环播放，加上这个属性，只播放一遍。

- android:drawable="@drawable/fight_01"，原来，`.png`是需要省略掉的。

- android:duration="100"，切换间隔时间100ms。

## 添加ImageView控件
1、打开activity_main.xml，Design，在Widgets中选中ImageView放到布局中。
2、双击，在src中选择Project>Drawable>frameanimation，OK。
3、指定一个id，添加的第一个ImageView默认为imageView，第二个默认为imageView2，依次类推。
4、拖拉一下，修改一下长宽和大小。

## 修改activity_main.xml

```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="0dp"
    android:paddingRight="0dp"
    android:paddingTop="0dp"
    android:paddingBottom="0dp" tools:context=".MainActivity">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="fitXY"
        android:id="@+id/imageView"
        android:src="@drawable/frameanimation"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentEnd="true"
        android:layout_alignParentBottom="true"
        android:layout_alignParentTop="true" />


</RelativeLayout>

```
这一步主要修改了一下ImageView的布局，使其填充整个界面，看起来效果比较好。非必要步骤，可不做。


## 修改MainActivity.java
```
package com.voidking.android.demo;

import android.graphics.drawable.AnimationDrawable;
import android.support.v7.app.ActionBarActivity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.widget.ImageView;


public class MainActivity extends ActionBarActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ImageView imageView = (ImageView) findViewById(R.id.imageView);
        final AnimationDrawable animationDrawable = (AnimationDrawable)imageView.getDrawable();

        imageView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                animationDrawable.stop();
                animationDrawable.start();

            }
        });
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
}

```
我们修改的，其实只有OnCreate函数，在里面添加了对ImageView的单击事件的监听和动作。
之所以先stop再start，是因为animationDrawable.start()之后，显示到了最后一个帧，停留在最后一个帧，但是，这时候animationDrawable的运行并没有结束，必须stop之后，才能开始新一轮动画播放。

## 运行
单击工具栏上的绿色三角形，选择一个AVD，运行即可。

# 效果展示
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/01.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/02.jpg)

# 代码分享
https://github.com/voidking/android-frameanimation ，需要的同学移步自取。


# 后记
一步一步，不怕慢，胜在不止！


# 参考资料
7天学会Android
http://www.duobei.com/room/trial/9152038430
