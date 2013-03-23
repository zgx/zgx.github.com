---
layout: post
title: "使用Python处理图像"
date: 2013-03-03 21:29
comments: true
categories: 
---

PIL(Python Imaging Library)是Pytho下的一个图像处理库，支持多种图片格式，提供
强大的图像处理和绘图功能。在[官网](http://www.pythonware.com/products/pil/index.htm)
下载即可使用，或者使用pip安装：
	pip install pil
	
<!--more-->
##使用Image模块

Image是PIL中的一个类，是图像的抽象。同时提供一系列工厂方法，包括从文件加载图像
、创建新图像。

引用Image模块
	from PIL import Image

从文件加载图像，并返回Image的实例：
	im = Image.open('filename.jpg')
	#这里可以传入文件夹，或者文件对象

###创建缩略图
这是我使用PIL的直接原因，优先学习。查了下Image的文档，有一个方法叫`thumbnail`,
我想我找到答案了。

**thumbnail(size, resample=0)**

功能：创建缩略图。本方法会修改对象自身，如果要保留原来的图片，请想调用Copy，保存下来。
返回：None。

参数：size:缩略图的大小，如：(100,75)。注意，这不是无条件转换的，比原图大的不转，
会保留图片的比例，实际缩小比例是：min(缩略长/原长, 缩略宽/原宽).

resample:重采样模式。支持模式有：NEAREST, BILINEAR, BICUBIC, or ANTIALIAS。

使用示例：

	from PIL import Image
	im = Image.open('mypic.jpg')   #加载图片，创建Image实例
	im.thumbnail((120,100), Image.ANTIALIAS)  #抗锯齿模式产生缩略图
	im.save('thumb_mypic.jpg')     #保存

小技巧：如果想要产生的缩略图的宽度固定，高度根据比例变化，可以吧高度设为一个很大值。如：
	im.thumbnail((100, 0xfffff))	#宽度为100px。除非高度大于0xfffff

###其他我用到的功能

下面简单介绍下PIL的其他技能。

**resize(size, filter)** => image

传入参数size为元组(width, height), filter的可以模式有：NEAREST，BILINEAR，BICUBIC，ANTIALIAS。

返回改变大小后的Image对象。这个跟缩略图不一样，这里的size是无条件的，会改变图片比例。

**rotate(angle)** => image
返回经过旋转处理的图片。

未完待续。。。

续集请查看文档：http://www.pythonware.com/library/pil/handbook/image.htm

###遇到的问题
处理jepg格式的文件时，报错"IOError: decoder jpeg not available"。
解决办法，安装libjpeg，然后重新安装PIL。

