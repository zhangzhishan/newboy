---
title: 那些显示对方ip信息的图片是怎么制作的？
tags: ip图片
categories: PHP
date: 2015-01-25 06:39:15
---

之前逛贴吧，总是能够看到很多很酷玄的签名，就如下面这张虽然很简陋，例如显示出你现在的位置啊，浏览器啦什么的。之前一直不知道是怎么弄得，后来发现原来php是可以搞定的。

那些显示对方ip信息的图片是怎么制作的呢？今天就由我带领着大家来制作一张吧，当然这些可以很炫酷的签名档了，如果想特别定制的话可以私信我，本来是想在自己博客上提供一个网页自动生成一些选项的，但是本着 **一切输入都是有害的**  的原则，没有做，见谅了。o(*￣▽￣*)ゞ

其实这个图片制作也是很简单的，下面是我的源码，也可以参看[Github](https://github.com/zhangzhishan/scripts/edit/master/informationpic.php)

其中ip地址是可以采用PHP直接获得的，然后地址查询API是用的[Netimpact](http://www.netimpact.com)提供的免费服务，每天只可查询250次，所以可能后面的就会出现不了结果的状况。


```php
<?php

$u_agent = $_SERVER['HTTP_USER_AGENT'];

$bname = 'Unknown';

$platform = 'Unknown';

$version = '';

if (preg_match('/linux/i', $u_agent)){

$platform = 'Linux';

}

elseif (preg_match('/macintosh|mac os x/i', $u_agent)){

$platform = 'mac';

}

elseif (preg_match('/windows|win32/i', $u_agent)){

$platform = 'windows';

}

if(preg_match('/MSIE/i', $u_agent)&amp;&amp;!preg_match('/Opera/i', $u_agent))

{

$bname = 'Internet Explorer';

$ub = 'MSIE';

}

elseif(preg_match('/Firefox/i',$u_agent))

{

$bname = 'Mozilla Firefox';

$ub = 'Firefox';

}

elseif(preg_match('/Chrome/i',$u_agent))

{

$bname = 'Google Chrome';

$ub = 'Chrome';

}

elseif(preg_match('/Safari/i',$u_agent))

{

$bname = 'Apple Safari';

$ub = 'Safari';

}

elseif(preg_match('/Opera/i',$u_agent))

{

$bname = 'Opera';

$ub = 'Opera';

}

$known = array('Version', $ub, 'other');

$pattern = '#(?&lt;browser&gt;'.join('|', $known).')[/ ]+(?&lt;version&gt;[0-9.|a-zA-Z.]*)#';

if (!preg_match_all($pattern, $u_agent, $matches)) {

}

$i = count($matches['browser']);

if ($i != 1) {

if (strripos($u_agent, "Version") &lt; strripos($u_agent,$ub)){

$version = $matches['version'][0];

}

else {

$version = $matches['version'][1];

}

}

else {

$version = $matches['version'][0];

}

if($version == null || $version=="") {

$version="?";

}

$ua = [

'userAgent' =&gt; $u_agent,

'name' =&gt; $bname,

'version' =&gt; $version,

'platform' =&gt; $platform,

'pattern' =&gt; $pattern

];

$browser = "Your browser: ".$ua['name']." ".$ua['version']." on ".$ua['platform'];

$img_number = imagecreate(850,60);

$backcolor = imagecolorallocate($img_number,125,185,222);

$textcolor = imagecolorallocate($img_number,255,255,255);

imagefill($img_number,0,0,$backcolor);

$agent=$_SERVER['HTTP_USER_AGENT'];

$ip=$_SERVER['REMOTE_ADDR'];

$host_name = gethostbyaddr($_SERVER['REMOTE_ADDR']);

$url = "http://api.netimpact.com/qv1.php?key=uiWMSWCvUGimIahk&amp;qt=geoip&amp;d=json&amp;q=$ip";

$d = file_get_contents($url);

$details = json_decode($d);

$data = explode(',' , $d);

$info = array(

'country_code' =&gt; $data[6] ,

'country_name' =&gt; $data[2] ,

'region_name' =&gt; $data[1] ,

'city' =&gt; $data[0] ,

'latitude' =&gt; $data[4] ,

'longitude' =&gt; $data[5] ,

'isp' =&gt; $data[3] ,

);

$messages = "Dear friends from $data[0], $data[1], $data[2]($data[4], $data[5]).";

$message2 = "Using $agent";

$time = date('Y-m-d H:i:s');

Imagestring($img_number,10,5,0,$messages,$textcolor);

Imagestring($img_number,10,5,20,$browser,$textcolor);

Imagestring($img_number,10,5,40,$time,$textcolor);

header("Content-type: image/jpeg");

imagejpeg($img_number);

?>
```