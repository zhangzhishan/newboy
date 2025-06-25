---
title: settingsync_vscode
tag: vscode
date: 2019-03-26 09:36:55
---

打算做这个插件了，基本思路构想的差不多。
一步步的完成，
第一步实现从vscode随便发送个内容到服务器或whatever，这一过程应该是最复杂的，因为目前对于typescript很不熟悉，不知道post过程会有多少坑，有网的时候查一下`async` `promise` `await` 等相关内容。在文件比较多的情况下，应该还是用异步比较靠谱，由于当前的直接是套用的github的一个api的库，所以直接包含了异步操作，这部分的话有两个方案，一个是参考`octokit/rest`这个库的实现，