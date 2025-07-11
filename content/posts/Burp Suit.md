---
title: XSS攻击——跨站脚本攻击与防御 第二章 发现XSS的工具集（1）Burp Suit
tags:
  - Burp
  - Web安全
  - XSS
  - 入门
  - 跨站脚本攻击
categories:
  - XSS
  - 安全
date: 2014-12-27 06:40:07
---

**介绍**

发现寻找XSS漏洞是一个很复杂以及消耗时间的任务，为了找到这些漏洞的位置，我们使用一大批的工具和技术。在这一章，我们探索那些作者在研究测试中发现的非常有价值的工具。

<!--more-->

注意许多XSS漏洞可以仅仅使用浏览器以及对细节的关注就可发现，这些都是主要在搜索框喝着类似的输入中。根据提交一个测试值给表单然后观察相应结果，你可以很快地发现这些简单的漏洞。然而，这些类似的漏洞你可以用一个Web扫描器在很短的时间内发现他们。一旦发现这些漏洞，工具就成了攻击过程中很宝贵的一部分。能够在百忙之中改变requests和response是发现一些有价值漏洞唯一的方法。我们应该注意这些工具并不仅仅可以用在发现XSS漏洞，在进行Web渗透测试中也很有用。

**Burp**

现代浏览器是为速度和效率而生的，这就意味着Web应用的安全评估就是一个很痛苦的任务，因为探究一个Web应用需要深层的分析，通常，为了测试一个应用，你想降低从服务器获得数据和发送数据到服务器的传输速度从而你可以阅读以及改变传输的数据。从而代理出现了。

在安全的早期，代理可以降低向外传输数据的链接速度，所以用户仅仅可以改变传输到服务器的数据；然而，这仅仅是分析Web应用的一部分。有时你非常应该改变接受的数据。例如，你想改变一个cookie使得它不使用Httponly或者移除一个Javascript功能。有时你想获得一个你浏览器发出的每一个请求的双向传输的细节。然后就出现了Burp Proxy(http://www.portswigger.net/burp/)（译者注：原文链接有误）

Burp Proxy是一个叫做Burp Suit的用于Web渗透的Java工具的一部分，但是对于本书的目的，仅仅一个功能是尤其有用的，那就是代理。上手，你需要安装Java的运行环境，你可以从Java的官方网站下载。一旦安装成功，你在你的浏览器改变你的代理设置，使用localhost或者127.0.0.1端口为8080.

图2.1chrome的代理服务器设置

![2014-12-27 05:16:01的屏幕截图](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-051601的屏幕截图2-300x150.png)

图2.2 Burp Suit的主窗口

&nbsp;

[![2014-12-27 05:16:21的屏幕截图](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-051621的屏幕截图-300x183.png)](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-051621的屏幕截图.png)

&nbsp;

一旦完成以上操作，你就可以启动Burp Proxy了，启动后是一个空白的窗口。 其中Intercept和Options窗口（是我们关注的最重要的窗口。首先，让我们来配置Burp Proxy来监控发送和接受的请求。在Options选项下取消勾选资源类型的限制，勾选Server Response拦截，以及取消勾选‘text’作为一个content type。这样将会展示你链接的所有服务器的所有的数据。

图2.3 Burp Suit Proxy Options配置屏

![2014-12-27 05:47:45的屏幕截图](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-054745的屏幕截图-300x205.png)

&nbsp;

Note：这也是一种识别你系统里面的间谍软件的方法，因为当有任何数据在你的客户端北修改之后，它讲停止并且警告你。如果你想查看你安装了什么间谍软件，你应该对于所有的客户端进行坚挺，因为每一个需要通过代理去展示什么在使用它。

&nbsp;

一但完成配置，你应该可以上网并且观察到从主机传入或者传出的所有数据。这既允许你查看传输的数据又允许你在你认为合适的时候进行修改。当然，你进行的任何修改在被传输给浏览器后会影响你并且值影响你，然而，如果它可以关闭javascript的保护，这就可以被用来做一些罪恶的事情，像持续型XSS，通常由于客户端方面的保护它并不被允许。同样的，在Asynchronous Javascript and 　XML（AJAX）的时代，这个工具对于监视和修改双向的传输数据也是很有力的，同时关掉客户端产生的任何防止浏览器进行修改的保护。

图2.4 请求监听

![2014-12-27 06:03:05的屏幕截图](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-060305的屏幕截图-300x197.png)

这也可以帮助移除一些有可能泄露给目标的信息，包括cookies， referrers或者其他一些不必须的信息，就如上图所示。另一个有用的特性是转换成16进制模式。特别是在你查看采用交替编码方式，像US-ASCII或者UTF-16.

在下面两幅图中，你可以看到，他们或者是不可见字符（null）或者是不在标准的ASCII码0-127的范围内而是在128-255的范围内。在这两种情况中，查看源文件将提供很少甚至无法提供他们采用的编码方法或者实施攻击所需要的特定字符集。（译者注：http://ha.ckers.org/weird/us-ascii.html网站已经被移除）

图2.5 作为16进制监控（us-ascii）

![2014-12-27 06:23:14的屏幕截图](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-062314的屏幕截图-300x237.png)

图2.6 作为16进制监控  (utf-16)

[![2014-12-27 06:21:35的屏幕截图](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-062135的屏幕截图-300x195.png)](http://code4fun.me/wp-content/uploads/2014/12/2014-12-27-062135的屏幕截图.png)

Burp Proxy是迄今为止最有用的手动评估Web应用的安全工具之一。他不仅可以帮助我们发现明显的问题，如果你知道你的目标的话它也可以进行规则配置。例如，如果你想仅仅寻找XML文件来调试AJAX应用，就可以创建一个仅仅获取该信息的规则。

最后，Burp仅仅是在大量的可以实现部分或者更多的Burp的功能的软件之一，但是没有别的软件以这种方式工作或者 有Burp Suit这么强大的能力。Burp Suit是一个用来理解HHTP实际工作过程的学习工具。

&nbsp;

&nbsp;