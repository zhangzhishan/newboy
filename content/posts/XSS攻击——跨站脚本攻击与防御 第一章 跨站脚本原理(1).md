---
title: XSS攻击——跨站脚本攻击与防御 第一章 跨站脚本原理(1)
tags:
  - Web安全
  - XSS
  - 跨站脚本攻击
id: 52
comment: false
categories:
  - XSS
  - 安全
date: 2014-12-15 05:59:42
---

注：此系列翻译自 [Seth Fogie](http://book.douban.com/search/Seth%20Fogie)的书籍《Cross Site Scripting Attacks》

本章主要内容：

跨站脚本的历史

Web应用安全

XML和AJAX介绍

<!--more-->

**介绍**

跨站脚本攻击可以追溯到1996年，万维网(World Wide Web)刚发明的时候。那是一个电子商务刚刚起步，页是网景、雅虎等公司的气泡时期。数以千计的网页正在采用HTML框架构建中。然后JavaScript就出现了，它导致了跨站的出现，永远改变了Web应用安全。JavaScript使得Web开发者可以创建采用图像轮转、浮动菜单以及弹窗等交互式网站。虽然和今天的Asynchronous JavaScript和XML（AJAX）应用标准比起来很普通，但是很快黑客就发现了这片新大陆。

黑客们发现，当可信任用户浏览网站时，他们可以在同一个浏览器窗口里强制加载任何网站（银行、拍卖、商店、邮箱等等）到一个HTML框架。然后他们可以跨过两个网站间的分界线并且从一个框架里读取数据到另一个。他们可以窃取存在HTML表格中的用户名与密码、cookies或者屏幕上的机密信息。媒体以浏览器漏洞报道了这个问题。网景公司作为当时主要的浏览器厂商实施了同源政策(same-origin policy)。同源政策限制了JavaScript从一个网站获取另外网站的数据。浏览器黑客们认为这是一个挑战，然后想出了很多聪明的办法攻破这个限制。

1999年12月，David Ross正在研究IE浏览器的安全相应问题。David受到也在寻找IE安全问题的Georgi Guninski工作的启发，他演示了网站内容的脚本注入（"Script Injection"），但是这个问题似乎只存在IE浏览器的服务端，在客户端并不存在。David在微软内部发表了一篇题为“Script Injection”的paper。这篇论文描述了这种情况，以及其发现过程，如何借助cookies攻击，跨站脚本（XSS）病毒如何运行，以及对此种情况的I/O过滤办法。

最后这个内容被分享到CERT(译者注：computer emergency response team)。他这么做的目的是以一种负责人的方式通知公众这种情况，从而使得在整个行业内的网站得到修复而不仅仅是微软的。再一次一月中旬的讨论中，这个组织从下面一系列相当幽默的建议中选择了"Cross Site Scripting"，

*   未授权网站脚本(Unauthorized Site Scripting)
*   非官方网站脚本(Unofficial Site Scripting)
*   URL参数脚本注入(Uniform Resource Locator(URL) Parameter Script Insertion)
*   跨站脚本(Cross-site Scripting)
*   合成脚本(Synthesized Scripting)
*   脚本欺骗(Fraudulent Scripting)

在2000年1月25日，微软在华盛顿的贝尔维尤的一个酒店约见CERT，相关厂商（例如Apache）和其余一些感兴趣的组织一起讨论这个概念。

David在Ivan Brugiolo、John Coates和Michael Roe的帮助下又重新写了这篇内部论文，以使得它适合公开。以和CERT协调，微软在2000年2月公开了这篇论文以及其他材料。过去几年的某天，这篇论文被从Microsoft.com上移除了，但是没有东西会在互联网上消失。我们现在可以在[http://ha.ckers.org/cross-site-scripting.html](http://ha.ckers.org/cross-site-scripting.html)找到他。

同时，黑帽子们为了感染用户在HTML聊天室、留言板、访客记录、邮件等任何地方提交包含HTML/JavaScript的文本，这就是这种攻击叫做HTML注入的来源。黑客们创造了一种基本的JavaScript恶意软件模型，他们提交这些代码以改变屏幕名称，显示讽刺信息，窃取cookies，改变网站页面颜色，显示病毒警告和其他一些不具有很明显恶意的危害。不久之后另一种攻击变种出现了，通过社会工程学，欺骗用户去点击一个特殊装饰的恶意链接也可以导致和HTML注入相同的结果，Web用户除了关闭JavaScript没有别的办法自卫。

那些年原来被认为是跨站脚本的漏洞变成了一个没有特殊名字的浏览器漏洞。HTML注入和恶意链接现在被分类为跨站脚本的变种，反射型跨站脚本和存储型跨站脚本。不幸的是这就是许多人弄不清这些术语的原因。更糟的是，"CSS"这个缩写与一个新诞生的浏览器技术Cascading Style Sheets弄混。最后在2000年早期，一个聪明人提议用“XSS”代替跨站脚本的缩写来避免混乱。然后就像我们知道的，它流传了下来，XSS有了自己的身份。几十篇描述他破坏性的论文和海量的漏洞利用建议充斥着网络。可很少有人听。

直到2005年，大量的安全专家和开发者都很少注意XSS。他们主要关注缓冲区溢出，僵尸网络，病毒，蠕虫病毒，间谍软件等等。同时全球每个月有100万新的网络服务器出现，他们有着脆弱的防火墙并且认为Secure Sockets Layer(SSL)是很奇怪的东西。更多的人认为使得XSS出现的JavaScript是一门玩具编程语言。“它不能获得操作系统的root权限或者exploit数据库，所以为什么我需要关心呢？点击一个链接或者参观一个网址能有多危险呢？”在2005年10月我们得到了答案。Samy Worm，第一个XSS蠕虫关闭了著名的社交网站MySpace。它的payload是相对温和的，Samy Worm被设计从一个MySpace用户的个人主页感染另一个用户，最终在短短24小时内感染了100万用户。安全世界惊醒了，然后JavaScript恶意程序的研究开始广泛开展起来。

短短几个月后，在2006年，JavaScript端口扫描器，攻破内网，按键记录，木马，浏览器历史记录窃取出现了并且产生了持续的影响。在主要网站的数百个XSS漏洞被揭露出来，犯罪分子开始以假冒身份来进行欺骗。毫不奇怪的，根基WhiteHat Security的记录，超过70%的网站现在是可利用的。MITRE的公共漏洞披露(CVE)项目是一个在商业软件或者开源软件中公开漏洞的集合，它标明XSS已经超过缓冲区泄漏成为被发现最多的漏洞。理所当然地，面对信息安全和电子商务，XSS成了潜在地最具有破坏性的漏洞。今天，当观众被问到是否听说过XSS时，几乎所有人都会举手。

&nbsp;