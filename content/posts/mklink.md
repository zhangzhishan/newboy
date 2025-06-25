---
title: mklink
tag: windows
date: 2019-04-30 16:42:05
---

[https://www.sinosky.org/mklink-cmd-useful-tips.html](https://www.sinosky.org/mklink-cmd-useful-tips.html)

create a symbolic from `Link` to `Target`:

```
mklink Link Target
```
`/D` can be used to create a link for directory:

```
mklink /D link target
```

Use `/H` when you want to create a hard link pointing to a file:

```
mklink /H Link Target
```
Use `/J` to create a hard link pointing to a directory, also known as a directory junction:

```
mklink /J Link Target
```