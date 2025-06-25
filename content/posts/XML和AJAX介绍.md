---
title: XSS攻击——跨站脚本攻击与防御 第一章 跨站脚本原理(3) XML和AJAX介绍
tags: xss
categories: 安全
date: 2014-12-19 04:15:23
---

## XML和AJAX介绍

我们假设本书的读者都有JavaScript和Html的基础，这两门技术都是基于一个被广泛传播很久的标准，已经有无数的关于他们是怎么工作以及你可以采用他们在网络上做什么的信息。然而，介于AJAX和XML(eXtensible Markup Language)刚刚被引入互联网世界，我们觉得对这两门技术进行一个基础的概述是很有必要的。

人们一般认为AJAX比XML更强一些，因为XML是AJAX的缩写的一部分。但是事实上并不是如此的。AJAX描述了一种可以有时悄悄进行web开发练习的方法。从基础上来说，AJAX是一组为了创建可以提高用户体验、可用性以及速度的Web应用的技术的集合。
在2005年这个词被主流Web开发者所使用，但AJAX的历史很悠久的。这门在今天被广泛使用的核心技术是由微软在开发远程脚本技术的时候首次使用。和传统的由DHTML定义的技术如IFRAME和LAYER比起来，由AJAX定义的这组技术是一个好的多选择。
AJAX的最基本的组成元素是XMLHttpRequest JavaScript对象。这个对象提供一种可以从远程服务器得到数据但是不需要刷新整个已经载入的页面的机制。根据使用的浏览器，这个对象可以产生很多不同的特色。XMLHttpRequest对象设计的很简单直观，下面的例子就是怎么发送请求以及使用请求。

```js
// instantiate new XMLHttpRequest
var request = new XMLHttpRequest;

// handle request result
request.onreadystatechange = function () {
    if (request.readyState == 4) {

        // do something with the content
        alert(request.responseText);
    }
};

// open a request to /service.php
request.open('GET', '/service.php', false)

// send the request
request.send(null);
```

由于各种原因，XMLHttpRequest对象并不能在所有浏览器上以相同的方式执行。这是由于AJAX是一门新技术，虽然他的标准很快就被制定了，但是仍然有很多我们需要解决各种浏览器的互斥问题的情况。这些问题通常是借助于AJAX的库来解决，但是作为安全研究，我们一般都要使用纯净的基础版本。
正如我们前面提到的，XMLHttpRequest对象是依赖浏览器版本的。例如微软的IE浏览器需要使用ActiveXObjext('Msxml2.XMLHTTP')或者甚至是ActiveXObjext('Microsoft.XMLHTTP')来创建和标准的XMLHttpRequest类似的对象。其余的浏览器也许有不同的方法来实现相同的事情，为了保证可以适配所有的浏览器的不同之处，我们通常喜欢用这里定义的函数：

```js
function getXHR () {
    var xhr = null;

    if (window.XMLHttpRequest) {
        xhr = new XMLHttpRequest();
    } else if (window.createRequest) {
        xhr = window.createRequest();
    } else if (window.ActiveXObjext) {
        try {
            xhr = new ActiveXObjext('Msxml2.XMLHTTP');
        } catch(e) {
            try {
                xhr = new ActiveXObjext('Microsoft.XMLHTTP');
            } catch (e) {}
        }
    }

return xhr;
};

// make new XMLHttpRequest objext
var xhr = getXHR();
```


表1.1总结了XMLHttpRequest对象的方法和属性
<!-- table.sample { border-width: 1px 1px 1px 1px; border-spacing: 2px; border-style: outset outset outset outset; border-color: gray gray gray gray; border-collapse: separate; background-color: white; } table.sample th { border-width: 1px 1px 1px 1px; padding: 1px 1px 1px 1px; border-style: inset inset inset inset; border-color: gray gray gray gray; background-color: white; -moz-border-radius: 0px 0px 0px 0px; } table.sample td { border-width: 1px 1px 1px 1px; padding: 1px 1px 1px 1px; border-style: inset inset inset inset; border-color: gray gray gray gray; background-color: white; -moz-border-radius: 0px 0px 0px 0px; } -->

abort()中止请求
<table class="XMLHttpRequest对象方法和属性">
<tbody>
<tr>
<th>方法/属性</th>
<th>描述</th>
</tr>
<tr>
<td>abort()</td>
<td>中止请求</td>
</tr>
<tr>
<td>getAllResponseHeaders()</td>
<td>以字符串的形式取回响应的头部</td>
</tr>
<tr>
<td>getResponseHeader(name)</td>
<td>取回特定名字的头部的值</td>
</tr>
<tr>
<td>setRequestHeader(name, value)</td>
<td>根据名字设定头部的值</td>
</tr>
<tr>
<td>open(method, URL, [asynchronous], [username, password])</td>
<td>设定使用的方法以及取回的网址，打开请求对象</td>
</tr>
<tr>
<td>onreadystatechange</td>
<td>这个属性可以获得当这个请求通过不同的状态时将要调用的事件的引用</td>
</tr>
<tr>
<td>readyState</td>
<td>readyState参数定义了请求的状态，可能的值有:
0 - 未初始化
1 - 打开
2 - 已发出
3 - 正在接收
4 - 加载完成</td>
</tr>
<tr>
<td>status</td>
<td>status属性返回响应状态码，如果请求成功则是200，如果需要重定向则是302，其他的状态码也是可能的</td>
</tr>
<tr>
<td>statusText</td>
<td>这个属性返回和状态码相关的描述</td>
</tr>
<tr>
<td>responseText</td>
<td>responseText属性返回响应体</td>
</tr>
<tr>
<td>responseXML</td>
<td>responseXML与responseText相似，但是如果服务器以XML作为响应，浏览器将将它转化为易存储的结构，这种结构通常被称为Documnet Object Model(DOM)</td>
</tr>
</tbody>
</table>
请注意responseXML和responseText的区别，他们都是返回响应体，但他们在功能方面确实有一些不同。

特别地，responseText被用于取回文本文档、HTML页面、二进制页面以及所有不是XML格式的网页。当我们需要处理XML的时候，我们使用responseXML属性，它会将响应文档解析成DOM对象。

我们已经展示了，responseText是怎么工作的，所以让我们看一下responseXML的应用。在提供另一个例子之前，我们先解释一下XML的作用。

XML是用来展示含义而不是像HTML那样展示结构，XML是一种具有具有自我描述性的没有作为的语言。其他与XML标准相关的是XPath, Extensible Stylesheet Language Transformation(XSLT), XML Schema Definition (XSD), Xlink, XForms, Simple Object Access Protecol (SOAP), XMLRPC等等。我们不能对这些全部进行讲解，因为这已经超出了本书 的范围，但是你可以在[www.w3c.org](http://www.w3c.org)上查阅他们。

虽然XML和HTML不同，但是他们都是由被称作元素和标签的块组成的。XML和HTML的元素都是高度结构化的。他们可以用一个树的结构表现，这通常被称作DOM。事实上，DOM是由World Wide Web Consortium定义的一组规格，它定义了XML结构是怎么被创建的以及他具有哪些方法属性，正如我们前面所说，HTML也可以被解析成DOM树。

其中一个最常使用的DOM函数是getElementsByTagName,它返回一个元素的数组。另一个流行的函数是getElementById，返回一个基于id的单独的元素。例如，借助JavaScript的帮助，我们可以轻松的提取所有的`<p>`元素并且用"Hello World"代替他们。例如:

```js
// get a list of all element
var p = document.getElementsByTagName('p');

// iterate over the list
for (var i = 0; i < p.lenght; i++) {
    // set the text of each to 'Hello World!'
    p[i].innerHTML = 'Hello World'
}
```


采用类似的方法，我们可以和XMLHttpRequest对象中的XML响应属性相互配合，就如前面描述的那样。例如：

```js
function getXHR () {
    var xhr = null;

    if (window.XMLHttpRequest) {
        xhr = new XMLHttpRequest();
    } else if (window.createRequest) {
        xhr = window.createRequest();
    } else if (window.ActiveXObjext) {
        try {
            xhr = new ActiveXObjext('Msxml2.XMLHTTP');
        } catch(e) {
            try {
                xhr = new ActiveXObjext('Microsoft.XMLHTTP');
            } catch (e) {}
        }
    }

    return xhr;
};
// make new XMLHttpRequest object
var request = getXHR();

// handle request result
request.onreadystatechange = function () {
    if (request.readyState == 4) {

        // do something with the content but in XML
        alert(request.responseXML.getElementsById('message'));
    }
};

// open a request to /service.xml.php
request.open('GET', '/service.xml.php', false)
// send the request
request.send(null);
// make new XMLHttpRequest objext
var xhr = getXHR();
```
如果服务器响应包括以下内容：

```HTML
<messageForYou>
        <overHere id="message">Hello World!</overHere>
</messageForYou>
```
浏览器就将在一个警告框里显示"Hello World!"。

理解基础的XML和AJAX是很重要的，它们已经成为了Internet不可或缺的一部分，它们对于理解这些技术对于传统Web应用的安全测试的影响也是很重要的。

## 总结

XSS是一种可以窃取敏感信息、劫持用户sessions、危机浏览器以及系统安全的攻击向导，XSS攻击自从Web早期就已经存在。今天他们对于电子商务产生了最大的威胁。