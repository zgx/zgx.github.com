---
layout: post
title: "Python抓取网页"
date: 2013-01-29 19:21
comments: true
categories: 
---
##真的很简单

最基本的抓取网页，真的很简单，一行代码就可以了

	import urllib2
	html = urllib2.urlopen('http://thesite.com').read()

没有异常发生的话，html就是抓取页面的源文件。

