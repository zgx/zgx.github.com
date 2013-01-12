---
layout: post
title: "Octopress技巧之设置关键字和描述"
date: 2013-01-11 21:39
comments: true
categories: 
description: 这一篇文章介绍了如何在Octopress框架下设置关键字和描述, 特别提到了首页设置关键字和描述的方法。
keywords: Octopress, 设置， 关键字， 描述
---
###关键字和描述

这里所说的关键字和描述是指网页`head`部分的元标签`meta`，是给搜索引擎看的，以此希望用户可以比较容易找到本站。
在html上就是面所示的标签：
<!--more-->
	
    <meta name="description" content="横竖弯钩是一个关注IT技术的博客">
    <meta name="keywords" content="IT, 编程, 码农的自嘲">

###Octopress设置关键字和描述的方法

那么在Octopress的模板是怎么在页面设置这两个字段的呢？我们先看代码。打开文件: `source/_include/head.html`。
找到设置“description”和“keywords”的地方：

    {% raw %}
    {% capture description %}
	  {% if page.description %}
		{{ page.description }}
	  {% else %}
		{{ content | raw_content }}
	  {% endif %}
	{% endcapture %}
    <meta name="description" 
		content="{{ description | strip_html | condense_spaces | truncate:150 }}">

    {% if page.keywords %}<meta name="keywords" content="{{ page.keywords }}">{% endif %}
    {%endraw%}

可以看出，“description”是取的是页面上的`description`字段, 就是在文件顶部被`---`包围起来的内容，关键字同样如此。
也就是说我们可以在每篇博客设置不同的关键和描述。

	---
	layout: post
	title: "文章的标题"
	date: 2013-01-11 22:00
	description: "在这里写上描述"
	keywords: 关键字1，关键字2
	comments: true
	categories: 
	---
	###你的博客
	你的博客正文

如上面的格式填上描述和关键字，Octopress帮你生成这一篇博客的页面就会包含你设置的关键字和描述了。如果你没有手动设置的话
，模板会帮你截取正文的一部分作为描述，至于关键字这个标签就没有了。

###首页如何设置

从模板代码我发现了，它没有对首页做特殊处理。通常我们需要在首页写上作为全站的描述和关键字，而模板默认的行为是在第一篇博客截取
一部分作为描述。这显然不是我们要的结果。

那怎么办呢？在这个问题我走了一些冤枉路。大致过程就是在配置文件`_config.yml`增加这两个字段，然后修改模板，判断如果是首页就
取全局配置的关键字和描述。

虽然改起来很简单，但是后来我发现了更简单的方法，在这里分享下。其实首页也是一个页面，也会有上面提到的普通文章页面的配置信息。
*只要修改index.html*就可以了。
打开source/index.html，如下设置：

	---
	layout: default
	description: "只是一个你们搜索引擎都没收录的网站，你说我是瞎折腾么"
	keywords: 技术，码农，单身, 单身，真的是单身
	---

OK，就怎么简单。不过不要学上面的堆砌关键字，话说谷歌百度之流不喜欢。

###举一反三
1. 对于网站其他页面，都可以用这个方法设置关键字和描述
1. 页面上的配置信息，我们可以添加自己的字段进去，在模板用`page.[字段名]`就可以访问了。这样你就可以给每篇加上额外的信息了。
   如`hide`:不显示；`forward`:转发。然后就可以在模板加上个性功能了。

