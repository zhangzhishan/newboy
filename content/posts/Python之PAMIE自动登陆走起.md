---
title: Python之PAMIE自动登陆走起
tags: Python
categories: Python
date: 2014-12-09 04:48:42
---

在Python中有一个功能是不得不说的就是Python的丰富的库，凡是你想要的基本都可以找到现成的库。

当然对于一个有极度追求效率的人，在我看来，用10min去解决一个要多次重复，每次重复时间会花费10s的问题是非常值得的，于是在很久之前要去下很多篇PPT的时候，就发现了这个PAMIE module，然后就顺利的花了一段时间解决了问题，很短的代码，就解决了大量重复的操作，今天帮人写一个自动登陆的脚本，又重新拿起这个module来，下面就详细说一下这个module。

当然第一步是官网[http://pamie.sourceforge.net/](http://pamie.sourceforge.net/)。

PAMIE是“Python Automated Module For I.E.”，也就是用来自动调用IE的一个模块，在Linux下使用就比较蛋疼了，然后尝试去google 是否有“similar module PAMIE in LInux”，然后发现没有任何结果，如果有人知道的话，欢迎留言。

然后下载地址是[http://sourceforge.net/projects/pamie/files/PAMIE/PAM3.0%20for%20Python%203.0/PAM3.0.zip/download](http://sourceforge.net/projects/pamie/files/PAMIE/PAM3.0%20for%20Python%203.0/PAM3.0.zip/download)，现在已经支持python 3，还是不错的。貌似由于这个需要win32com.client 支持，所以如果直接放入python库的话，会报错，经过搜索发现只需要下载Python extensions for Windows即可，下载地址[http://sourceforge.net/projects/pywin32/files/?source=navbar](http://sourceforge.net/projects/pywin32/files/?source=navbar)

貌似没有找到有官方文档支持，其实也不需要用官方文档，因为我们完全可以直接看源文件，里面的PAM30.py函数定义都可以根据名字猜出是干什么的，例如buttonExists(self, name) 肯定是检测是否有名字是button的键的，其中name的值可以是utton的id，name，value或者index，需要使用的时候我们直接审查一下元素，看这个元素的名字到底是什么就可以了。

下面是一个简单的自动登陆示例。

```python
#!/usr/bin/env python

# coding=utf-8

# Author = huhuhushan

from PAM30 import PAMIE

ie = PAMIE()

url = "你要打开的网址"

ie.navigate("url")

ie.setTextBox("userName1", 'username')

ie.setTextBox('password1', 'password')

imagesrc = ie.getImageValue('random', 'src')

randtext = imagesrc[-4:]

ie.setTextBox('rand', randtext)

ie.clickImage("/images/bot_lodin.gif")
```

当然这个过程可能，有些变量名字会改变，不过都是很简单的。

比较坑爹的地方就是ie升级后，这个调用API的时候就会报错，`TypeError: getElementsByTagName() takes 1 positional argument but 2 were given`。

貌似从ie9 开始就出现问题了，不过经过测试发现采用兼容模式是可以运行的。