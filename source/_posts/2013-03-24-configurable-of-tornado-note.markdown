---
layout: post
title: "tornado源码阅读笔记——Configurable"
date: 2013-03-24 03:29
comments: true
categories: 
---
Configurable类位于tornado.utils模块，继承它的类的可以承担工厂方法的职责，可以根据配置或环境决定示例化那个子类。

在tornado中，IOLoop便是继承Confifurable的的一个例子。IOLoop的职责有定义操作接口和根据说在的操作系统决定实例化那个子类，
如下图，Linux支持epoll，BSD支持kqueue，而在Windows就只能选择Select了。用户在使用IOLoop时，是需要IOLoop接口，
而不必关注实际使用的是那个实现，而样写出来的代码可移植性就更好了。
<!--more-->

![ULM图](http://bcs.duapp.com/picfile/2013/3/76f7caa0b2bcfde0.png)

继承Configurable的类应该实现如下方法：

* configurable\_base. 这是一个类方法，一般返回
* configurable\_default. 类方法。此方法由子类实现，应当返回一个恰当的类型。这个就是工厂方法实现的地方。
* initialize. 实例方法。实例后由Configurable调用，代替__init__的作用。

下面是Configurable的__new__方法，Configurable的魔法正是由这个方法执行的。
    def __new__(cls, **kwargs):
        base = cls.configurable_base()
        args = {}
        if cls is base:
            impl = cls.configured_class()
            if base.__impl_kwargs:
                args.update(base.__impl_kwargs)
        else:
            impl = cls
        args.update(kwargs)
        instance = super(Configurable, cls).__new__(impl)
        # initialize vs __init__ chosen for compatiblity with AsyncHTTPClient
        # singleton magic.  If we get rid of that we can switch to __init__
        # here too.
        instance.initialize(**args)
        return instance

下面我们看下IOLoop是如何实现工厂的职责的。
IOLoop一般只用一个实例，下面是获得IOLoop的全局单例的方法，可以看到这里并没有为IOLoop选择具体的实现。

    @staticmethod
    def instance():
        if not hasattr(IOLoop, "_instance"):
            with IOLoop._instance_lock:
                if not hasattr(IOLoop, "_instance"):
                    # New instance after double check
                    IOLoop._instance = IOLoop()
        return IOLoop._instance

下面来看 configurable方法，很简单，就是返回IOLoop类。

    @classmethod
    def configurable_base(cls):
        return IOLoop

接着看configurable\_default方法，这里就是IOLoop根据平台，选择具体实现的地方了。

    @classmethod
    def configurable_default(cls):
        if hasattr(select, "epoll") or sys.platform.startswith('linux'):
            try:
                from tornado.platform.epoll import EPollIOLoop
                return EPollIOLoop
            except ImportError:
                gen_log.warning("unable to import EPollIOLoop, falling back to SelectIOLoop")
                pass
        if hasattr(select, "kqueue"):
            # Python 2.6+ on BSD or Mac
            from tornado.platform.kqueue import KQueueIOLoop
            return KQueueIOLoop
        from tornado.platform.select import SelectIOLoop
        return SelectIOLoop
