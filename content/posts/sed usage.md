---
title: sed usage
tag: cpp
date: 2019-04-30 16:42:37
---

[sed 简明教程 | | 酷 壳 - CoolShell](https://coolshell.cn/articles/9104.html)

# s command

`sed "s/before/after's/g" filename.txt` 这个命令可以进行字符串替换并输出

如果需要修改文件内容的话可以用下面两个命令

    sed "s/before/after's/g" filename.txt > filename.txt
    sed -i "s/before/after's/g" filename.txt

这里的 `s` 和 `vim`中的使用基本相同，所以也就可以利用其余的一些正则符号。如：

    sed 's/^/#/g' filename.txt
    sed 's/$/ --- /g' filename.txt
    sed "3,33s/a/b/g" filename.txt #替换3到33行之间的文本
    sed "s/\<a/b/1" filename.txt #只替换每一行的第一次出现且a处于词首
    sed "s/a\>/b/2" filename.txt #只替换每一行的第二次出现且a处于词尾
    sed "s/a/b/3g" filename.txt #只替换第一行的第3次出现及以后的单词
    sed "s/a/$&$/g" filename.txt # 用&当作被匹配的变量

注： `\<`表示词首, `\>`表示词尾。

`\\\`表示需要查找和替换的backslash，也即反斜杠，相应的有很多对应的正则符号需要加上 `\`。

## 多个匹配

    sed '1,3s/a/b/g; 3,$s/c/d/g' filename.txt # 第一行到第三行a->b，第三行及以后c->d
    sed -e '1,3s/a/b/g' -e '3,$s/c/d/g' filename.txt #与上式等价
    

## 圆括号匹配

 圆括号括起来的正则表达式所匹配的字符串会可以当成变量来使用，sed中使用的是 `\1,\2`

    sed 's/This is my \([^,&]*\),.*is \(.*\)/\1:\2/g' my.txt

# N commands

把下一行的内容纳入当成缓冲区做匹配

    sed 'N;s/my/your/' pets.txt

通过该命令可以实现只替换奇数行

# other commands

a — append

i — insert

c — replace

d — delete

p — print