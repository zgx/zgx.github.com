---
layout: post
title: "把博客放在Github"
date: 2013-01-10 09:09
comments: true
categories: 
---
故事是这样的，我开始想要在vim写博客，找到Markdown是写技术博客的很好用的一种标记语言，
同时发现Github Pages可以托管用Markdown写的博客站点，然后就尝试在Github写博客。
<!--more-->

###Markdown
Markdown 是一种轻量级标记语言，创始人为John Gruber和Aaron Swartz。它允许人们“使用易读易写的纯文本格式编写文档，
然后转换成有效的XHTML(或者HTML)文档”。这种语言吸收了很多在电子邮件中已有的纯文本标记的特性。
Github的README.md文件就是用Markdown格式写的。在这里[了解Markdown语法](http://hswg.info/blog/2013/01/01/markdown/)的语法，学习10分钟就可以用它写博客了，是不是很简单。

###Github Pages
然后很自然的了解到了[Github Pages](http://pages.github.com/)。Github设计了Pages功能，允许用户自定义用户首页，和项目首页。也就是说，
用户可以把静态页面托管到Github。当然，Github Pages的功能不止是托管静态页面，否则用html写博客，同时还要关注样式那是无法接受的。
Github Pages同时提供了模板功能，可以用解析引擎是Jekyll。

一句话教程：
> 在gh-pages分支add一个index.html文件，push到github，访问`http://username.github.com/projectname/`就可以看见你发布的网页了。

很重要一点，Github Pages支持[绑定域名](https://help.github.com/articles/setting-up-a-custom-domain-with-pages)。

###Jekyll
Jekyll是一个静态站点生成器，它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能，不需要数据库，文章可以使用Markdown、HTML、Textile格式的文件保存。
Github可以解析符合Jekyll规范的网站源码。

使用上面说的仨，你就可以建立很酷的博客了，这个博客的特点是：
> - 完善的版本管理，你可以找回你每一次新增、修改的信息。
> - 在Github你可以有免费、无限的流量。
> - 使用你喜欢的编辑器写博客，使用简洁的Markdown，不必在忍受在线编辑器。
> - 绑定自己的域名。
> - 当然，这样做很酷。

###Octopress
使用上面的方案你就可以DIY你个人博客的每一部分。
从零开始不是多数人的选择，你可以参考开源的[Jekyll模板](https://github.com/mojombo/jekyll/wiki/Sites)。
当然，你还可以选择Octopress。使用Octopress的方案也用到了上面的仨，不一样的地方在于Jekyll生成静态页面的过程发生在本地。使用它提供的发布命令`rake deploy`即可发布到github。
这样做带来的好处有：
> - 有一系列便捷的命令负责生成、预览、发布、新建文章、新建页面等等。
> - 有本地预览功能。
> - 有一套还算美观的模版，这个模板修改起来还是很方便的。
> - 网站源码和静态页面分离。可以在git以不同分支管理。

搭建Octopress参考[官方文档](http://octopress.org/docs/)即可，在Linux下搭建还是很简单的。
在windows下，我的选择是在Cygwin下搭建，
中途出了点小问题，如果你要这样做，建议参考下：
[在Cygwin搭建Octopress](http://hswg.info/blog/2013/01/07/octopress-on-cygwin/),
这是在windows下的一个选择。直接在Windows安装也是可以的，只是我不喜欢使用Windows那烂得不行的Cmd。

###社会化评论
大家或许想到了，静态页面怎么能有评论功能？一个答案就是社会化评论系统。
社会话评论系统的好处是方便管理，降低网站复杂度，分享功能有助网站推广。
坏处也很明显，数据不可控。当下，社会化评论是一个趋势。
[这是](http://hswg.info/blog/2013/01/09/using-social-comment/)我在Octopress使用社会化评论的过程。

###域名
访问部署在github的页面使用的网站是：`http://username.github.com/projectname/`。你也可以绑定自己的域名。
在Github Pages使用域名很方便。步骤如下：

1. 让网站根目录，也就是gh-pages分支的根目录，新建一个叫`CNAME`的文件,里面写上自己的域名。
1. 如果绑定顶级域名，新建A记录指向`204.232.175.78`（这是Github的[文档](https://help.github.com/articles/setting-up-a-custom-domain-with-pages)给出的，也许已经改变）
1. 如果绑定二级域名，修改CNAME指向`username.github.com`即可。

###再补充一点
按照Github的[文档](https://help.github.com/articles/user-organization-and-project-pages)，
建立一个与用户名相同的仓库，把页面传到master分支，就可以用`http://username.github.com/`访问，
不过我试了下，没有成功(404)，有谁能指导下吗？

好嘞，博客搭建完成。请在[这里][myblog]看下效果：[http://hswg.info][myblog]

如果你有兴趣，也可以参考阮一峰的博客:
[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

对了，这篇文章就是用Markdown写的。

[myblog]: http://hswg.info/ "横竖弯钩的博客"

