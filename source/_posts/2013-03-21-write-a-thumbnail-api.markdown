---
layout: post
title: "用Python在SAE写一个生成缩略图的API"
date: 2013-03-21 21:38
comments: true
categories: 
---

其实这是一个无聊的工具，此API有几大功能：

1. 通常会让你的图片加载变慢
1. 消耗我的云豆
1. 你不需要自己保存缩略图

<!--more-->
![二雪人](http://xtask.sinaapp.com/thumbnail.png?src=http://bcs.duapp.com/picfile/2013/1/2eaccc638a524c63.jpg&width=300&height=300)

调用示例：
> http://xtask.sinaapp.com/thumbnail.png?src=http://bcs.duapp.com/picfile/2013/1/2eaccc638a524c63.jpg&width=300&height=300

参数说明：

- src：原图片地址
- width:最大宽度
- height：最大高度
- 注：不会改变比例，不会把图片变大。

基于Tornado写的，简单贴下代码，有问题可以留言哦。

    class ThumbMaker(RequestHandler):
        def get(self):
            try:
                srcUrl = self.get_argument('src', None)
                if not srcUrl:
                        raise Exception
                width = int(self.get_argument('width', 0xfffff))
                height = int(self.get_argument('height', 0xfffff))

                rsp = urllib2.urlopen(srcUrl)
                imgIo = StringIO(rsp.read())
                im = Image.open(imgIo)
                #if im.mode != "RGB":
                #       im = im.convert("RGB")
                im.thumbnail((width, height), Image.ANTIALIAS)
                imgOut = StringIO()
                im.save(imgOut, 'png')
                self.set_header('Content-Type', 'image/png')
                self.write(imgOut.getvalue())
            except Exception, e:
                print str(e)
                self.set_header('Content-Type', 'image/png')
                self.write(open('nopic.png').read())
            return
