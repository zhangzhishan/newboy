---
title: Linux bash快捷键
tags:
  - bash
  - Linux
id: 49
comment: false
categories:
  - Linux
date: 2014-12-14 03:55:00
---

<pre>[来自网络]bash快捷操作

生活在 Bash shell 中，熟记以下快捷键，将极大的提高你的命令行操作效率。

编辑命令

    Ctrl + a ：移到命令行首
    Ctrl + e ：移到命令行尾
    Ctrl + f ：按字符前移（右向）
    Ctrl + b ：按字符后移（左向）
    Alt + f ：按单词前移（右向）
    Alt + b ：按单词后移（左向）
    Ctrl + xx：在命令行首和光标之间移动
    Ctrl + u ：从光标处删除至命令行首
    Ctrl + k ：从光标处删除至命令行尾
    Ctrl + w ：从光标处删除至字首
    Alt + d ：从光标处删除至字尾
    Ctrl + d ：删除光标处的字符
    Ctrl + h ：删除光标前的字符
    Ctrl + y ：粘贴至光标后
    Alt + c ：从光标处更改为首字母大写的单词
    Alt + u ：从光标处更改为全部大写的单词
    Alt + l ：从光标处更改为全部小写的单词
    Ctrl + t ：交换光标处和之前的字符
    Alt + t ：交换光标处和之前的单词
    Alt + Backspace：与 Ctrl + w 类似

重新执行命令

    Ctrl + r：逆向搜索命令历史
    Ctrl + g：从历史搜索模式退出
    Ctrl + p：历史中的上一条命令
    Ctrl + n：历史中的下一条命令
    Alt + .：使用上一条命令的最后一个参数

控制命令

    Ctrl + l：清屏
    Ctrl + o：执行当前命令，并选择上一条命令
    Ctrl + s：阻止屏幕输出
    Ctrl + q：允许屏幕输出
    Ctrl + c：终止命令
    Ctrl + z：挂起命令

Bang (!) 命令

    !!：执行上一条命令
    !blah：执行最近的以 blah 开头的命令，如 !ls
    !blah:p：仅打印输出，而不执行
    !$：上一条命令的最后一个参数，与 Alt + . 相同
    !$:p：打印输出 !$ 的内容
    !*：上一条命令的所有参数
    !*:p：打印输出 !* 的内容
    ^blah：删除上一条命令中的 blah
    ^blah^foo：将上一条命令中的 blah 替换为 foo
    ^blah^foo^：将上一条命令中所有的 blah 都替换为 foo
</pre>

&nbsp;