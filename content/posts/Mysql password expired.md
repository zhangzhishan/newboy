---
title: Mysql password expired
tags: Mysql
date: 2 Dec 2016 7:01 PM
---

今天登录网页，突然出现了这个问题

```
    Your password has expired. To log in you must change it using a client that supports expired passwords.
```

其实解决方法很简单，只需要登录到服务器中的 Mysql，然后修改root 的密码就可以了

```
    mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password', 'root'@'localhost' PASSWORD EXPIRE NEVER;
```
