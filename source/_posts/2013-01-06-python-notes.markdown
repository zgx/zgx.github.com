---
layout: post
title: "Python笔记"
date: 2013-01-06 20:22
comments: true
categories: 
---
一些个人关于python的笔记
<!--more-->
python源文件包含非ASCII字符时，需要包含编码声明，文件是utf-8时，声明如下
	# -*- coding: utf-8 -*-

tornado处理上传的文件
	class Upload(RequestHandler):
		def post(self):
			if self.request.files and 'picfile' in self.request.files:
				fobj = self.request.files['picfile'][0]
				#fobj是一个字典，键值有：filename, body, content_type
				#根据情况处理fobj就是了
