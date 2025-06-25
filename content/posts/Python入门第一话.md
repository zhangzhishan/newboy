---
title: Python入门第一话
tags: Python
categories: Python
date: 2014-12-08 02:20:07
---

在程序猿界一直流传着这样一个未解之谜，就是如果你学一门语言的话，如果你的第一个程序不是"hello,world!"的话是不可能学会这么语言的。

当然，这是一个笑话，不过学习语言，我们当然还是从最基础的打印输出开始。

正如，前言提到的部分，我们是采用的Python的语法，也许Python 3 最坑爹的地方就是，把小白的第一步print函数给修改了，于是给大家造成了很大的困惑，就见各种贴吧论坛上，各种纳闷，为什么我的print出错了啊，只因为是用着Python 3 的编译器，跑着Python 2 的程序。下面就是Python3的"hello,world!"

```python
print("hello, world!")
```
需要注意的是Python 不是像其他语言，例如C 用大括号‘{}’，作为程序的分隔符，而是采用四个空格或者制表符（也就是Tab键）来进行分隔。并且空格和制表符不可以混用，如果混用的话就会报错，当然你要统一一种选择，这个具体的用哪一种是没有强制要求的，不过建议使用四个空格。最开始的时候，Python这种语法特性，可能会让人别扭，不过时间长了之后，你就会发现这种语法的美感，写出来的都是很清晰易于阅读的代码。不像C一样，对与'{'的位置一直无法统一。

知道了，print之后，下一个问题就是我们print不可能总是直接print那句话，当我们需要print很长一句话的时候，这样就会降低代码的可读性，或者多次输出的时候。例如下面的代码就丑到爆。

```python
print('Python is an easy to learn, powerful programming language. It has efficient high-level data structures and a simple but effective approach to object-oriented programming. Pythonâs elegant syntax and dynamic typing, together with its interpreted nature, make it an ideal language for scripting and rapid application development in many areas on most platforms.'）
```

所以这就产生了找个短的东西来代表这一长串字符的，于是我们就可以尝试着去做一下。

```python
pythonIntro = 'Python is an easy to learn, powerful programming language. It has efficient high-level data structures and a simple but effective approach to object-oriented programming. Pythonâs elegant syntax and dynamic typing, together with its interpreted nature, make it an ideal language for scripting and rapid application development in many areas on most platforms.'
print(pythonntro)
```

然后我们发现这样做是合理的，看来我们的猜测方法是正确的。

我们没有去告诉Python 这个后面的东西是数字还是字符串，他就自己处理了，这也就是他和其他一些语言的区别，我们不需要大喊一声，“哎，编译器，这有一个变量，他的名字是pythonIntro， 他是指的是一句话，这句话内容是balabala ”，我们只需要简单的指定他的值就好，至于是数字还是一句话就交给Python来处理好了，不需要我们操心了。

那一串短的东西，也就是叫做变量，故名思议是可以改变的，至于叫什么名字完全由你决定，但是要注意这个取名也是有学问的，如果你写出下面的程序，估计别人看到只会说一声WTF。

```python
apple = 'orange'
print(apple)
```

开始可能我们喜欢拿一些简单的字母a，b什么的，可是当程序长了，这样你到后面就没法知道这是在干什么了，具体的命名方法有很多，可以自行百度。