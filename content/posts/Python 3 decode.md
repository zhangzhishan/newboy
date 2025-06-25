---
title: 九浅一深 Python 3 编码问题
tags: Python
categories: Python
date: 2015-01-29 05:49:05
---

Python 3 与 2 有一个很大的不同就是编码问题，这个问题也导致了很多之前的程序没法很好的运行了，尤其是在网络方面，之前的各类示范代码都会报错，会出现一些类似下面的错误。

```
>>> u'hello' + b'world!'
TypeError: Can't convert 'bytes' object to str implicitly
```

这个还不是最坑的，还是可以接受的，下面的错误才是莫名奇妙，

```
>>> import urllib.request
>>> print(urllib.request.urlopen('http://code4fun.me').read().decode('utf-8'))
nicodeEncodeError: 'gbk' codec can't encode character '\xbb' in position xxxx
```

并且是时好时坏，由于电脑是双系统，所以有的时候会在 Windows 下写一些代码，然后有些本来好好的代码，就会抽风，然后报上面的错误。这个的只要原因是 Windows 会默认编码方式是 'gbk'，对此目前还没有找到很好的办法解决，可以参看[stack overflow](http://stackoverflow.com/questions/3218014/unicodeencodeerror-gbk-codec-cant-encode-character-illegal-multibyte-sequen)。

这个简直是不能忍了，于是现在是彻底放弃 Windows 写 Python 的想法，有的时候不得不用 Windows 的时候就选择 SSH 到 Linux 然后继续在 Linux 下写一些代码，然后有些本来好好的代码，就会抽风，然后报上面的错误。

废话说了一堆，下面就说一下 Python 3 中的编码问题，从 Python 3 开始，存在两种形式 text 和 binary data，这个代替了 Python 2 中的 Unicode strings 和 8-bit strings。 所以当我们在 Python 3 中正常的写一个字符串时默认是 Unicode 格式的，也就是 Python 3 中的 str 类型。

```
>>> type('code4fun')
<class 'str'>
>>> type(u'code4fun')
<class 'str'>
>>> type(b'code4fun')
<class 'bytes'>
```

而 Python 2 中则是只有前面加了 'u' 之后才是unicode编码的，

```
>>> type('code4fun')
<type 'str'>
>>> type(u'code4fun')
<type 'unicode'>
>>> type(b'code4fun')
<type 'str'>
```

相信大家可以从上面这两段运行结果可以清晰的发现区别，也就是在 Python 3 中用 str 类型来存储文本，而用 bytes 类型来存储数据。

当然，我们难免会接触到很多 bytes 需要转化为 str 或者反之的情况，这时候就需要引入两个函数 decode() 以及 encode() 了。

decode() 也就是解码的意思，何谓之解码呢， Unicode 就和 ASCII 一样都是一种编码方式，由于 CPU 能识别的只是01010，而人类的语言是由字母和笔画组成的，于是就在不同的字符和二进制数字（十六进制和二进制类似）之间有一个对应关系，最简单的是 ASCII 表，只有127个，当然汉字没在里面，就连法语、德语等需要 'ö' 此类字符的国家也没有包括起来，于是就开始进行扩增，经过各种协调统一之后就有了 Unicode 而 UTF-8 就是其中的一种包含常用字符且不占据很多空间的编码方式。

上面说那么多，其实就是为了让大家理解 unicode 和 bytes 之间的关系，从而梳理清楚 decode() 和 encode() 的关系。一堆 bytes 就是一堆二进制的形式的字符人类无法理解，所以我们需要对他们进行解码从而可以得到我们可以理解的形式，而字符串编码之后就可以得到计算机可以理解的 bytes 类型。

```
>>> print('呼呼呼山'.encode())
b'\xe5\x91\xbc\xe5\x91\xbc\xe5\x91\xbc\xe5\xb1\xb1'
>>> print(b'\xe5\x91\xbc\xe5\x91\xbc\xe5\x91\xbc\xe5\xb1\xb1'.decode('utf-8'))
呼呼呼山
```

## 最后做个总结

计算机方便理解的是 bytes 类型，而人类方便理解的是 str 类型，那么我们就可以方便的构造一个三明治模型，

![unicode sandwich](/images/unicodesan_1538722256_26901.png)

外面与计算机接触的部分是 bytes，而我们自己处理的部分是 unicode，一方面 Python 3 的默认为 unicode，更加方便我们处理，另一方面我们也可以直观的理解，不需要多次乱七八糟的转化。

## Reference
[Pragmatic Unicode](http://nedbatchelder.com/text/unipain.html)
[Text Vs. Data Instead Of Unicode Vs. 8-bit](https://docs.python.org/3.0/whatsnew/3.0.html#text-vs-data-instead-of-unicode-vs-8-bit)
[Dive Into Python 3](http://woodpecker.org.cn/diveintopython3/strings.html)