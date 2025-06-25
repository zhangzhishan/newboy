---
title: 让你的Mac也可以不走锐捷（Npucat for ＊nix）
categories: Python
date: 2015-08-19 22:04:58
tags: Python
---

鉴于之前的西工大的猫只有windows版本，为了部分＊nix用户，于是重新修改了一下代理程序，由于程序较长就不贴出来了，下载链接如下([百度云][1])。

由于是用Python写的，而＊nix系统基本都已经装好Python，所以把下载好程序，进入当前文件夹运行下面命令

```
nohup python npucatnix.py >npu.log 2>&1 &
```

或者

```
nohup python npucatnix.py > /dev/null 2> &1 &
```

第一个命令会把产生的log文件保存到当前目录，一般也没什么用，所以推荐第二个命令。

然后对于Mac，设置如下。

打开设置－网络

[![屏幕快照 2015-08-19 下午9.38.50][image-1]][2]

&nbsp;

选择高级

[![屏幕快照 2015-08-19 下午9.39.23][image-2]][3]

&nbsp;

选择web代理，服务器为127.0.0.1 端口为1024。

然后你就可以尽情的上网了

youku

[![屏幕快照 2015-08-19 下午10.02.41][image-3]][4]

B站

[![屏幕快照 2015-08-19 下午10.04.02][image-4]][5]

[1]:	http://pan.baidu.com/s/1kT4D7x1
[2]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%889.38.50.png
[3]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%889.39.23.png
[4]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%8810.02.41.png
[5]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%8810.04.02.png

[image-1]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%889.38.50.png
[image-2]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%889.39.23.png
[image-3]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%8810.02.41.png
[image-4]:	http://code4fun.me/wp-content/uploads/2015/08/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2015-08-19-%E4%B8%8B%E5%8D%8810.04.02.png