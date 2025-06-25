---
title: jsPDF 那些坑
categories: javascript
date: 2015-08-24 16:29:35
tags: javascript
---

# jsPDF 那些坑

前段时间由于工作需要，需要把之前网页上的打印报名表功能改成打印成pdf，于是Google之，发现了[jsPDF](https://parall.ax/products/jspdf)这个js插件，本以为可以快快的解决问题，却发现里面真是太多坑了。

## 引用

这个很简单只需要，从[Github](https://github.com/MrRio/jsPDF)上下载源代码，然后你会发现里面是包括着各种库文件以及多个测试，当然这些都不是必须的，我们只需要引用里面的dist中的一个文件以及两个插件即可。如下

```html
<!-- Scripts down here -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js"></script>
<script src="http://html2canvas.hertzen.com/build/html2canvas.js"></script>

<!-- Code editor -->

<script src="https://cdn.jsdelivr.net/ace/1.1.01/noconflict/ace.js" type="text/javascript" charset="utf-8"></script>

<!-- Scripts in development mode -->
<script type="text/javascript" src="dist/jspdf.min.js"></script>
 ```

这时候坑就出现了，因为里面的依赖的jQuery库是用的Google的cdn，所以我们就跪了，还是乖乖换成百度的cdn或者存成本地吧。

```html
<script src="http://libs.baidu.com/jquery/1.8.3/jquery.min.js"></script>
```

## 打印输出

由于我的主要应用是打印页面上的html文件，所以在此就以此举例。要打印的html文档部分如下：

```html
<p class="my_show">code</p>
<p class="my_show">4</p>
<p class="my_show">fun</p>
```
对于网页有两个函数可以使用一个是fromHTML另一个是addHTML，但是比较悲剧的事fromHTML不支持中文，所以我们就只能使用addHTML这个函数了，直接打印的函数很简单，如下，不过就是这个函数导致了后面的诸多坑。

```javascript
var pdf = new jsPDF('p', 'mm', 'a4');
var print_content = $('.my_show');
var filename = '准考证.pdf';
pdf.addHTML(print_content,function() {
    pdf.output('dataurlnewwindow');
    pdf.output('save', filename);
});
```
## 分页
在这举例的html文档比较短，完全可以容纳到一页里面，但是在实际使用中的html内容比较多，不可能容纳到一个页面里面，但是这样打印，并不会分页，只打印一页的内容，这样怎么破呢？官方给的方案是使用参数'pagesplit'，示例如下。

```js
var pdf = new jsPDF('p', 'pt', 'a4');
var options = {
         pagesplit: true
    };

pdf.addHTML($(".pdf-wrapper"), options, function()
{
    pdf.save("test.pdf");
});
```
但是采用这个生成的pdf的分辨率兼职时惨不忍睹，这能干毛啊。于是考虑多次调用addHTML来生成，首先在浏览器的console中进行测试，发现OK，于是开心的码出了下面的代码，以为可以解决问题。

```js
for (var i = print_content.length - 1; i <= 0; i--)
{
  pdf.addPage();
  pdf.addHTML(print_content[i], function(){});
};
pdf.output('dataurlnewwindow');
```
结果没有成功，于是就蛋疼了，在console里明明测试成功，为什么就是不行呢，后来发现是用for循环的问题，不用for循环的时候就OK的，这实在想不通了，后来经过多次测试，估计是因为addHTML函数的时候，可能还没有把内容打印出来，就已经运行到输出了，所以才会生成空白的文件，于是果断添加了个延时进行测试，然后可以正常生产pdf了，可是这样太慢了，又想到了采用循环调用的方法，这也是最后的解决方案。如下：

```js
function printbypage(pdf, k){
  if(k <= $('.my_show').length)
  {
    pdf.output('dataurlnewwindow');
  }
  pdf.addHTML($('.my_show')[k], function(){
    if(k &lt; $('.my_show').length - 1)
    {
      pdf.addPage();
    }
    printbypage(pdf, k + 1);
  });
};

function printmyshow() {
  var pdf = new jsPDF('p', 'mm', 'a4');
  printbypage(pdf, 0);
};
```
完美解决问题。

## 小坑

*   当打印生成背景为黑白的时候，只需要设置打印元素的背景颜色为白色。
*   一些国外的脚本引用最好都更换为本地的，以防止不稳定。
*   坑爹的不支持IE9以下，好在数字公司，企鹅公司等的浏览器具有chrome内核，不过有的时候会默认为兼容模式，所以可以采用meta标签让其使用chrome内核，如下:
    * 若页面需默认用极速核，增加标签：`<meta name=”renderer” content=”webkit” />`
    * 若页面需默认用ie兼容内核，增加标签：`<meta name=”renderer” content=”ie-comp” />`
    * 若页面需默认用ie标准内核，增加标签：`<meta name=”renderer” content=”ie-stand” />`
