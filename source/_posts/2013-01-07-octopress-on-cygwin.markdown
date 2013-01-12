---
layout: post
title: "在Cygwin搭建Octopress"
date: 2013-01-07 15:03
comments: true
categories: octopress 
---
用了半天时间在Ubuntu搭建了octopress，跟着octopress的[文档](http://octopress.org/docs/setup/)一步一步来就可以了。
当我在本机的cygwin环境重复这一操作的时候，出现了一个小问题，记录一下。下面是操作步骤：
<!--more-->
1. 直接用Cygwin setup.exe安装Ruby，目前的版本就是文档要求的1.9.3，OK。
1. 安装bundler
		gem install bundler
1. 进入octopress目录，执行bundle install
		cd octopress
		bundle install
  问题出现了，关键提示如下：
		posix-spawn.c:9:19: 致命错误：spawn.h：No such file or directory
1. 上面的问题是由于posix-spawn的bug引起的，需要自己编译安装：
		gem install rake-compiler -v 0.7.6
		git clone git://github.com/rtomayko/posix-spawn.git
		cd posix-spawn
		rake gem
		gem install pkg/posix-spawn-0.3.6
1. 再执行`bundle install`就没用问题了。
1. 享受octopress吧.

