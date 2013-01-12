---
layout: post
title: "在Otcopress使用社会化评论"
date: 2013-01-09 00:15
comments: true
categories: 
---

*在octopress中使用disqus很简单*，只要修改在配置文件“_config.yalm”：
	# Disqus Comments
	disqus_short_name: your_short_name	#在这里输入你在disqus注册时申请的short name
	disqus_show_comment_count: false
<!--more-->
octopress的默认模板就会检测到`disqus_short_name`配置的存在，就会把Disqus评论插件显示出来了。
在文件`source/_layouts/post.html`,下面的代码就是负责把Disqus显示出来的。
	{% if site.disqus_short_name and page.comments == true %}
	  <section>
		<h1>Comments</h1>
		<div id="disqus_thread" aria-live="polite">{% include post/disqus_thread.html %}</div>
	  </section>
	{% endif %}
	
![disqus](http://bcs.duapp.com/picfile/2013/1/de86c60b1e7b51f1.png)
这是默认配置下的Disqus的界面，可以出是英语的，目前语言也没有中文可配置。不过对我们博客的访客来说很大问题。
关键的问题是它支持社交平台都是墙外之物，或许以后Disqus跟它们混了，点了这伙就跑出来了：
![404](http://bcs.duapp.com/picfile/2013/1/575181cb7965ec37.png)

目光转向国内。而目前业界提供这类方案的网站有友言(jiathis旗下产品)、评论啦、多说、Denglu评论(灯鹭旗下产品)。
最早听说的是“多说”，先*使用多说*。先上图：
![多说](http://bcs.duapp.com/picfile/2013/1/ff0d42b3f55937b0.png)

国内土产就是不一样，中国特色主流社交帐号都可以登录，可以无障碍的在社交平台分享对博客来说还是很有意义的。
Octopress当然没有对多说内置支持，不过在模板添加进去还是很容易的。在刚才的Disqus插件代码上面，添加如下代码。
	{% if page.comments == true %}
	<!-- Duoshuo Comment BEGIN -->
		<div class="ds-thread"></div>
		<script type="text/javascript">
		var duoshuoQuery = {short_name:"your_short_name"};
		(function() {
			var ds = document.createElement('script');
			ds.type = 'text/javascript';ds.async = true;
			ds.src = 'http://static.duoshuo.com/embed.js';
			ds.charset = 'UTF-8';
			(document.getElementsByTagName('head')[0] 
			|| document.getElementsByTagName('body')[0]).appendChild(ds);
		})();
		</script>
	<!-- Duoshuo Comment END -->
	{% endif %}

多说也是使用了short_name这个字段，short_name需要你到[多说](http://duoshuo.com/)申请。我们可以参考Octopress的方法使用配置和插件的方式来使用多说，不过过度设计不是我的风格。
这样就赶上潮流，使用了一把设计化评论。

