---
title: 离线安装深度学习框架
tags: deep-learning
date: 16 Jan 2017 6:20 PM
---
由于有台电脑不能连接互联网，且其中的有些数据需要应用到深度学习框架来进行分类，经过好几天的实践终于完成了。下面是实践出来的几种方法。
## Docker

这是我最终采用的方法，因为这个是最简单省事的。Docker 产生的目的是帮助我们一次配置，处处使用，作为一个容器技术，可以很方便的完成我们的需求，由于我们的采用的是 Windows 主机，就以 Windows 为例。

### 安装 Docker

在 Linux 系统下安装 Docker 是很简单的，添加好 Docker 的 APT 仓库，然后直接执行
` sudo apt-get install docker-engine`
即可。具体的可以参照官网。

在 Windows 下可以使用 [Docker Toolbox][1] 这个包括运行 Docker 所需的一切组件的集合，根据提示操作就可以安装好。在安装好好软件之后，启动 Docker 的命令行，会提示你需要打开*硬盘虚拟化*，根据操作系统进入 BIOS 即可，这时候就可以进入 Docker 界面了。

### 联网获取 Docker 镜像
当然，我们需要联网获取 Docker 镜像，先找一台可以联网的电脑，然后拉取相关镜像，把镜像导出，再把镜像导入就可以了。本以为是很简单的一件事情，结果实践起来，状况百出。

首先是各种网络问题，国内局域网的力量是强大的，各种无法连接服务器或者是网速很慢，无奈之下，好在还有一个国外的服务器，索性去服务器上 pull 下来，然后再本地拉取服务器上的镜像。

第二是 Keras 的各种镜像水平良莠不齐，有的是版本过低，有的是缺少一些必须库，多次尝试无果之下，只得重新写了一个CPU版本的 [Dockerfile][2]，可以直接采用下面命令运行，即自动拉取镜像并运行了。
`docker run -it -p 8888:8888 -p 6006:6006 zhishanzhang/keras-dl bash`

### 导出导入镜像
到达这一步就很简单了，导出镜像。
`docker save -o zhishanzhang/keras-dl > keras-dl.tar `
把镜像导入
`docker load -i keras-dl.tar `

### Just Enjoy It
这时候可以继续运行之前的命令，做自己的工作了。
P.S.：在 Docker 运行命令中可以使用 -v 参数把文件夹映射如 Docker Container 中，如下面命令。
`docker run -it -p 8888:8888 -p 6006:6006 zhishanzhang/keras-dl -v ~/test:/root/test bash`



[1]:	https://www.docker.com/toolbox
[2]:	https://github.com/zhangzhishan/keras-dl