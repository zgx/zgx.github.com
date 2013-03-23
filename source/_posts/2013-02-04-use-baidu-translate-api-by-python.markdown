---
layout: post
title: "Python使用百度翻译API"
date: 2013-02-04 18:30
comments: true
categories: 
---
谷歌了一下，发现谷歌翻译API竟然要收费。退而求其次次，用Python写了个调用百度翻译API的工具。
<!--more-->

###使用前提

你要有百度的`api_key`。如果你玩过百度的运环境（BAE），你已经有了。随便找一个应用
的API KEY就可以用了。
如何没用，你可以你可以在[这里](http://developer.baidu.com/wiki/index.php?title=%E5%B8%AE%E5%8A%A9%E6%96%87%E6%A1%A3%E9%A6%96%E9%A1%B5/%E7%BD%91%E7%AB%99%E6%8E%A5%E5%85%A5/%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)
了解如何获得app key。顺便还可以玩一下BAE。

###百度翻译API

**URL**
	http://openapi.baidu.com/public/2.0/bmt/translate

**请求参数**

- from. 源语言语种：语言代码或auto
- to. 目标语言语种：语言代码或auto
- client\_id. 上面说的api\_key
- q. 待翻译内容

**返回内容**
JSON字符串。示例如下：
	{"from":"en",
	 "to":"zh",
	 "trans_result":[{"src":"today","dst":"\u4eca\u5929"}]}

trans\_result就是我们需要的翻译结果了。注意trans_result是数组，可能会没有或者有多个结果返回吧，
不过目前我还没碰到过。

###Python调用示例
在Python使用这个API很简单，简单的就是范围URL，获取结果就是了。下面的代码比较偷懒，权当参考。

	# -*- coding: utf-8 -*-
	import urllib2
	import urllib
	import json

	DU_TRANS_URL = 'http://openapi.baidu.com/public/2.0/bmt/translate?from=%s&to=%s&q=%s&client_id=%s'
	DU_CLIENT_ID = 'yourkeyyourkeyyourkeyyourkey'

	def trans(slang, tlang, trans_str):
		du_url = DU_TRANS_URL % (slang, tlang, urllib.quote(trans_str), DU_CLIENT_ID)
		try:
			rsp = urllib2.urlopen(du_url)
		except Exception, e:
			raise Exception('Connecting to server fail:' + str(e)) 
		rs = json.load(rsp)
		if 'error_code' in rs:
			raise Exception('trans error:' + rs['error_msg'])
		if rs['trans_result']:
			return rs['trans_result'][0]['dst']
		else:
			raise Exception('No result')

	def c2e(src):
		return trans('zh', 'en', src)

	def e2c(src):
		return trans('en', 'zh', src)

	if __name__ == '__main__':
		print c2e('这是中文')

我们的工具箱又多了一个翻译功能了。哈哈哈。
