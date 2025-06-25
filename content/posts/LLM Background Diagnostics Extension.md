---
title: LLM Background Diagnostics VSCode Extension
tags:
  - vscode
date: 2025-06-08 23:12:27
---

因为最近连续遇到两个之前code 的隐藏bug被触发的情况，而在debug的过程中，把相关部分code直接发给大模型都能正确找到bug，所以想有没有个vscode extension可以直接对于已有代码检查呢？ 于是让Gemini和ChatGPT分别做了调研，可是没有找到满足需求的插件。  
既然没有，那就自己搞吧，vibe coding起来。然后就用 Roo Code 写了起来，没想到竟然一次就过了，然后又迭代了几轮小的需求，也都一次完成了。  
所以就有了这个插件: [LLM Background Diagnostics Extension](https://marketplace.visualstudio.com/items?itemName=zhangzhishan.llm-background-diagnostics)。  

![[example.png]]

然后试用了下，发现效果还是不错的，用之前有隐藏bug的代码试了下，都可以发现这些bug。目前实现的功能其实很粗糙，还有以下问题：  
- 实现比较粗暴，直接发送了整个当前文件给LLM，所以当文件太大的时候可能会有context的问题，目前来看似乎还可以。  
- 因为只是发送了当前文件，所以有些跨文件的bug其实是无法发现的，主要针对的是当前文件内各函数的实现。  
- 由于LLM返回的行号信息不对，所以虽然根据内容做了一些校验，但是仍然会存在一些问题和实际不太对应的地方。  
- 误报。  
- 不支持个人 API。因为公司有GitHub copilot，所以就直接调用了 vscode llm api，不需要额外配置就可以直接用了。模型选择了个人用起来感觉效果最好的 Gemini 2.5，但是如果用量比较大的话会 rate limit。