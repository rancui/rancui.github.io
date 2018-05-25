---
title:  html a 标签实现打电话，发短信功能
subtitle: html5，css3
date: 2016-10-09 10:20:00
thumbnail: https://raw.githubusercontent.com/liyina91/bolgImages/master/a_href.png
categories: Web开发
author:
  nick: liyina91
  github_name: liyina91
tags: [Load]
---

前些时间做移动端项目时，偶然发现a标签的带参数发送短信功能在安卓和ios下的写法是有区别的，稍不注意，可能会造成不必要的bug，趁空闲时间总结下笔记，大家一起互相学习。

下面就给大家详细讲一下如何实现和解决实际工作中可能遇到的问题:
### 关于打电话、发短信功能的实现
```
	<a href="tel:10086">打电话给：10086</a>
	<a href="sms:10086">发短信给：10086</a>
```


 实际在项目中应用时，发短信功能有时候需要带参数文案发送，安卓和ios的写法如下:

 <font color="#5c969d">安卓系统上面的写法是：</font>

```
	<a href="sms:10086?body=你好Android">给 10086 发短信</a>
```

 <font color="#5c969d">ios系统上面的写法是：</font>

```
	<a href="sms:10086&body=你好iphone">给 10086 发短信</a>
```
### 做项目时应用demo


```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1,minimum-scale=1 maximum-scale=1, user-scalable=0">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <meta name="format-detection" content="telephone=no">
    <title>示例</title>
    <style>
	   a{display: block;margin:10px;}
    </style>
  </head>

  <body>
	<div class="tel">
	  <a href="tel:10086">给10086打电话</a>
	  <a href="sms:10086">给10086发短信</a>
	  <a href="sms:10086?body=你好Android" id="js-sms">给 10086 发短信</a>
	</div>
	<script>
	  var browser={
	    versions:function(){
            var u = navigator.userAgent, app = navigator.appVersion;
            return {         //移动终端浏览器版本信息
                trident: u.indexOf('Trident') > -1, //IE内核
                presto: u.indexOf('Presto') > -1, //opera内核
                webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
                gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
                mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
                ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
                android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器
                iPhone: u.indexOf('iPhone') > -1 , //是否为iPhone或者QQHD浏览器
                iPad: u.indexOf('iPad') > -1, //是否iPad
                webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
            };
         }(),
         language:(navigator.browserLanguage || navigator.language).toLowerCase()
	 }
		  
	//默认是给安卓发短信 判断为iphone后 改变写法
		  if(browser.versions.iPhone==true){
			document.getElementById('js-sms').setAttribute('href','sms:10086&body=你好iphone')
		  }
	//	document.writeln("语言版本: "+browser.language);
	//	document.writeln(" 是否为移动终端: "+browser.versions.mobile);
	//	document.writeln(" ios终端: "+browser.versions.ios);
	//	document.writeln(" android终端: "+browser.versions.android);
	//	document.writeln(" 是否为iPhone: "+browser.versions.iPhone);
	//	document.writeln(" 是否iPad: "+browser.versions.iPad);
	//	document.writeln(navigator.userAgent);

	</script>
  </body>

</html>	
```

如需转载，请注明出处~~









