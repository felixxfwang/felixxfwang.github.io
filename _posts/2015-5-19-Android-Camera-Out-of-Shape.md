---
layout: post
category: Android
title: 关于Android Camera开发中的形变问题总结
tags:
  - Android
  - Camera
  - SurfaceView
---

前几天在做一个Android项目，要录制视频上传到服务器，在使用Camera和SurfaceView的时候遇到了各种奇怪的形变问题，今天在这里做一下总结。

## 三个尺寸

1. Surfaceview的尺寸  
Surfaceview是用来预览Camera的，当它全屏时就是Screen的大小。

2. Picturesize/VideoSize  
这是拍照或录像后的照片或视频的尺寸。

3. Previewsize  
这是预览时帧数据的尺寸。

## 三种变形

1. 预览画面的物体长宽失真。
2. 拍照后照片上物体变形/录像后视频影像变形。
3. 点击拍照瞬间，预览画面上会突然变形然后恢复到正常预览/录制视频期间，预览画面变形，录制视频结束后，恢复正常预览。

第一种变形的原因是Surfaceview和Previewsize的长宽比率不一致导致的预览画面失真。因为Camera在显示时会将预览帧数据缩放成Surfaceview大小，当比率不一致必然会物体变形。至于这个缩放算法不同手机会有区别。

第二、三两种变形都是因为Previewsize和Picturesize/VideoSize的长宽比率不一致所致。

总之一句话，为了保证图片或视频不失真，要保证预览Surfaceview、Previewsize、Picturesize/VideoSize的长宽比率要一致。如果Surfaceview是全屏的话，要获取屏幕长和宽得到比率。

## 一个陷阱

Android的Camera默认是横着拍照的，如果你要竖着拍照，需要运用

{% highlight java %}
Camera.setDisplayOrientation(90);
{% endhighlight %}

来使预览图像变成竖直的。如果是录像时，还要运用

{% highlight java %}
MediaRecorder.setOrientationHint(90);
{% endhighlight %}

让录制出来的视频旋转90度以正常播放。

这时候即使你保证预览Surfaceview、Previewsize、Picturesize/VideoSize的长宽比率一致了，仍会奇怪地发现预览图像仍然是扭曲的（除非你所有的长宽比都设置为1：1，但是有些手机是不支持宽高相等的PreviewSize的）。这又是为什么呢？

这个问题困扰了我很久，后来发现原来你将预览图像旋转了90度变成竖直的之后，相应的PreviewSize的宽就变成了高，高变成了宽，而我设置PreviewSize时用的方法是

{% highlight java %}
param.setPreviewSize(optimalPreviewSize.width, optimalPreviewSize.height);
{% endhighlight %}

如果长宽比不为1：1，图像肯定是变形的。所以给Camera设置PreviewSize的时候应该这样写：

{% highlight java %}
param.setPreviewSize(optimalPreviewSize.height, optimalPreviewSize.width);
{% endhighlight %}

说白了，在所有用到previewSize的地方都要将宽高对调使用。

这样预览图就不会再变形了。
     

