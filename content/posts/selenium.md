---
title: selenium 自动登陆 ——浏览器在手，天下我有。
tags: Python
categories: Python
date: 2014-12-13 21:32:17
---

在之前的一篇文章中写过关于ie自动登陆的PAMIE模块的使用。

当时就是在纠结，在Linux下怎么办呢？一个机缘巧合的机会遇到了selenium。

废话不多说了，[官网](https://pypi.python.org/pypi/selenium)

安装还是很简单，直接用pip就好。
```
	pip install -U selenium
```
或者下载下来，然后在根目录运行
```
	python setup.py install
```
安装好后，先来小试一把。
```
	from selenium import webdriver
	browser = webdriver.Chrome()
	browser.get('http://code4fun.me')
```
然后并没有很爽快的开始进行下去， 而是出现了一个错误。

```
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
  File "/usr/local/lib/python2.7/dist-packages/selenium/webdriver/chrome/webdriver.py", line 59, in __init__
    self.service.start()
  File "/usr/local/lib/python2.7/dist-packages/selenium/webdriver/chrome/service.py", line 66, in start
    "ChromeDriver executable needs to be available in the path. "
selenium.common.exceptions.WebDriverException: Message: ChromeDriver executable needs to be available in the path. Please download from http://chromedriver.storage.googleapis.com/index.html and read up at http://code.google.com/p/selenium/wiki/ChromeDriver
```

原来是没有安装chromedriver，那好下载链接已经给出来了，那就乖乖去下载好了，系统是elementary 64位的， 那就乖乖选择64位的咯，下载下来把文件放到路径里面去就好了。

```
sudo mv -f chromedriver /usr/local/share/chromedriver
sudo ln -s /usr/local/share/chromedriver /usr/local/bin/chromedriver
sudo ln -s /usr/local/share/chromedriver /usr/bin/chromedriver
```

就是先把文件移到share文件夹下，然后再从其中几个路径用ln，链接过去，其实当然也可以每个地方都复制一遍。

把这些都移动过去之后，我们重复上一次的命令，就可以愉快的看到自动打开了浏览器，并且进入了我们想进入的网站。