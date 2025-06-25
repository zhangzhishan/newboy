---
title: 个人思绪汇集
tag:
date: 2020-05-09 19:59:18
---

随着互联网技术的发展, 我们不断在网上留下各种线索, 可能在豆瓣里标记了一本刚看过的书, 又在微博上针对于当下热点新闻发出了自己的评论, 但是这些分散到四处, 就比较烦了. 好在 telegram channel 是一个比较开发的平台, 且 telegram 上有各种各样的 Bot. 这里借助 [RSSHub](http://rsshub.app/) 和 telegram 的 [rssbot](https://github.com/iovxw/rssbot) 将个人的相关信息都推送到 telegram channel 里面.

## RSSHub
此部分相当于是我们的基础, 里面有广大网友们开发的各种神奇的路由. 由于很多服务有反爬虫的设置, 所以我们为了更好的使用, 最好是自己在服务器上搭建.根据官方文档, 操作起来也比较简单, 这里就不赘述了. 这里有各种部署方案, [install](https://docs.rsshub.app/install/).

以下是我用的几个RSS订阅：

```
Code4Fun (http://code4fun.me/atom.xml)
Twitter Likes - tangtanghill (http://rsshub.app/twitter/likes/tangtanghill)
hillshan 的 Twitter (http://rsshub.app/twitter/user/tangtanghill)
zhangzhishan's GitHub repositories (http://127.0.0.1:1200/github/repos/zhangzhishan)
zhangzhishan’s starred repositories (http://rsshub.app/github/starred_repos/zhangzhishan)
呼呼呼山的微博 (http://127.0.0.1:1200/weibo/user/2622464373)
```

## RSSBot
为了可以把单独的RSS信息聚合到一个 channel, 这里找了一个 telegram 的bot订阅所有信息, 然后推送到相关的 channel. Bot的话由于一直在断断续续的学习 Rust, 所以就选了个用 Rust 写的. 不过确实用起来还是很稳定. 由于官方, 直接由 build 好的, 直接下载下来二进制文件就可以了.

我们只需要根据[telegram 官方文档](https://core.telegram.org/bots#3-how-do-i-create-a-bot), 建立一个bot, 然后设置几个相应的命令就好了.

```
/rss       - 显示当前订阅的 RSS 列表
/sub       - 订阅一个 RSS: /sub http://example.com/feed.xml
/unsub     - 退订一个 RSS: /unsub http://example.com/feed.xml
/export    - 导出为 OPML
```

`/sub` 命令除了直接订阅到Bot外, 当你把该Bot加到某个 Channel 之后, 可以通过 `/sub channelid url` 这个命令把通知直接订阅到某个 Channel中去.

![sub to channel](/images/20200509220713780_11570.png)

这里当我们需要 ChannleID, 这个的获得也非常方便, 我们可以随便转一条 Channel 中的内容到 [@getidsbot](https://t.me/getidsbot) 即可, 结果如下图.

![getidsbot](/images/20200509220950847_16848.png)

这里就可以得到我们的ID了. 然后就可以方便的把RSS更新 push 我们的 channel 了.


> [呼呼呼山](http://code4fun.me)
> 2020-05-09 19:59:18

