---
title: Rust 使用 Sqlite
tags: rust
date: 2019-01-14 11:11:49
---

主要是记录一下在Windows平台下利用Rust使用Sqlite相关操作的一个过程，别的语言应该也大同小异。
## Rust相关设置
在`cargo.toml`的`dependencies`中添加引用`Sqlite3`的库[rusqlite](https://github.com/jgallagher/rusqlite)，

```
rusqlite = "0.16.0"
```
具体的使用直接参考官方文档。但这时候如果直接运行`cargo run`会报错如下，因为缺少`sqlite3.lib`



## 本地build
为了本地build，首先在官网下载windows 的[Precompiled binaries](https://sqlite.org/download.html)，本文以x64环境为例。

下载到本地解压缩会看到有两个文件，`sqlite3.def`和`sqlite3.dll`，下面需要对这个文件进行