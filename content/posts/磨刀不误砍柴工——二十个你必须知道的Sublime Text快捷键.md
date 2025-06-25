---
title: 磨刀不误砍柴工——二十个你必须知道的Sublime Text快捷键
tags: Sublime Text
categories: 碎碎念
date: 2015-01-21 18:24:26
---

有点标题党，纯属娱乐。

作为其他编辑器一派的，Sublime Text绝对是个不错的选择，虽然没有Emacs那么强大，也没有Vim那样专注，但是在Windows下绝对是个不错的选择，插件和可配置性还是很强大的。（Emacs没试过，不过Windows下的Vim配置着实不爽）

## ctrl + shift + p

如果要在Sublime里面选择一个最强大的命令，那绝对是这个，这个快捷键可以显示当前文档所有可用的命令或者设置（包括插件）, 自动生成函数块之类的。

下面是演示的python自动新建一个类的过程。

&nbsp;

[![ctrlshiftp](http://code4fun.me/wp-content/uploads/2015/01/ctrlshiftp.gif)](http://code4fun.me/wp-content/uploads/2015/01/ctrlshiftp.gif)

## ctrl + r 和 ctrl+g ctrl + p

这三个是相互可以转化的，所以放在了一起。

ctrl+g是跳转到指定行， ctrl + r 是函数名查找，ctrl + p是 文件搜索，但是如果把ctrl+g出现的冒号删除就成了ctrl+p的功能，若换成@就实现了ctrl+r的功能。

## ctrl + b

这个也是很强大的功能，函数写完当然要进行编译了，这个就是用来进行编译的，里面内设了一些常用语言的编译，然后你也可以自己设定，由于日常有些工作需要matlab，所以我就设置了一个matlab的编译，下面是我的设置，其余的可以参考这个，自行解决。

``` 
// Change path to matlab.exe per local settings
"cmd": ["F:/Video/MATLAB R2013b/bin/matlab.exe", "-nodesktop", "-nosplash",
"-r", "\"run('$file')\""],
"selector": "source.m",
"working_dir": "${project_path:${folder}}"
```

## ctrl + / 注释，不多说了
## ctrl+[ 或者 ]  代码的缩进调整

## 下面是对变量的一些操作：

### ctrl + 左右移动键 每次移动一个变量

### alt + 左右移动键 每次移动一个单词（如果有下划线分开，就会停止）

### shift + 左右移动键 随着移动会对那些字进行选择（可以和ctrl以及alt，home, end等联合使用）
   
   shift 和一些移动键的结合也是相当强大的，可以直接进行选择编辑。
   
### ctrl + d 选择变量
   
   多次按可以进行同一变量的多个选择
   
### ctrl + delete 从该处删除单词直到词尾
   
### f3 向后搜索单词（联合shift向前索引，alt 表示 find all）
   
### ctrl + m 在两个配对的（大中小）括号之间跳转
   
### ctrl + space 自动补全选择

## 多行编辑：

### shift + enter 在该行上面插入一行

ctrl + enter 在该行下面插入一行

### ctrl + l选择整行

### ctrl + alt + 上下键 选择多行，方便进行多行的更改
### ctrl + home / end回到全文文首 / 末尾

## 多文件，多窗口：

### ctrl + page up / down 查看上 / 下一个文件

### alt + shift + 数字 多窗口，数字是窗口的个数（在此时ctrl + 数字会跳转到第几个窗口）
### alt+数字 跳到第几个文件（类似chrome）

## one more thing: f11 全屏 更加专注于代码