---
layout: post
title: "ubuntu下搭建代理服务器"
date: 2013-04-20 20:30
comments: true
categories: 
---

##安装squid
	sudo apt-get install squid

##squid配置

squid的配置文件在：`/etc/squid3/squid.conf`。这个文件有很详细的说明，
不过对于我来说是太啰嗦了。记下几个配置要点，以后照着操作就是了。

找到`http_access deny all`，将`deny`改成`allow`。意思是允许任何IP使用这个代理。
	http_access allow all


配置端口，这是服务器的监听端口，也就是客户端连接服务器时配置的端口，这里保持默认。
	http_port 3128

缓存大小设置
	cache\_mem 32 MB #vps的配置文件在内存很小，还是节省点吧。

重启squid
	sudo service squid restart

然后在客户端设置代理测试即可。

现在这个配置是运行任何IP匿名访问的，可以配置基于用户名密码的访问控制，目前用不到，
以后再写了。
