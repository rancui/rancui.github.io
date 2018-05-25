title: 移动web开发适配
subtitle: H5页面的兼容适配
date: 2016-9-28
thumbnail: https://raw.githubusercontent.com/rancui/images/master/shipei.png
categories: Web开发
author:
  nick: 一念rancui
  github_name: rancui
  
tags:
  - 项目总结

---


## 说到移动web开发，自然而然的会想到H5，那什么是H5呢？
---
H5是HTML5（HyperText Markup Language 5）超文本标记语言第五次重大版本的简称，标准通用标记语言下的一个应用，也是一种规范和标准。

HTML5本身并非技术，而是标准。现在国内普遍说的 H5 是包括了 CSS3，JavaScript 的说法。

“超文本”就是指页面内可以包含图片、视频、链接、音乐、程序等非文字元素。

2014年10月29日，万维网(W3C)联盟宣布，经过接近8年的艰苦努力，该标准规范终于制定完成。

### HTML5 中的一些有趣的新特性：
<font color="#5c969d">
绘画

本地音频视频播放

动画

地理信息

硬件加速

离线缓存（即使在没有网络的情况下）

本地存储

从桌面拖放文件到浏览器上传

语义化标签
</font>



## 一.移动开发过的H5前端项目
---

目前为止，我们团队已做过的基于微信和hybrid开发的前端项目有：

<font color="#5c969d">卡卡贷第一版，星星钱袋，学历贷，丽人贷，豆豆钱，豆豆房租，豆豆花，金薪贷，赎楼贷，安家派，手机贷，简历贷，秒分，vcs（PC端贷后催收系统），dashboard以及一些项目的推广活动页。</font>

## 二.移动开发的适配
---
### 1.采用rem单位
众所周知，目前苹果手机的屏幕尺寸有4种，但安卓手机的屏幕碎片化却很严重，各种屏幕尺寸，这给我们在做适配的时候也带来了一定的麻烦。为了尽可能的做到全机型的屏幕适配，我们CSS的单位采用的是rem混合px，更主要的是采用rem来做适配，那么什么是rem? 为什么要使用rem？它和px之间又是怎样的一种关系呢？


<font color="#5c969d"> rem（font size of the root element）是指相对于根元素的字体大小的单位。简单的说它就是一个相对单位。计算的规则是依赖根元素，这也给px和rem换算带来了很大的便利性。</font>


rem这是个低调的css单位，近一两年开始崭露头角，有许多同学对rem的评价不一，有的在尝试使用，有的在使用过程中遇到坑就弃用了。但从一些大厂的同行的反馈以及我们自己的实践来看，rem综合评价是用来做webapp最合适的人选之一。

腾讯手Q的一些推广活动页和淘宝m站也使用了rem：<a href="https://m.taobao.com/#index" target="_blank">m.taobao.com</a>

做适配的时候，我们可以采用CSS3中的媒体查询@meidia screen and (min-width:xxx) and (max-width:xxx) 单位是px来针对不同范围的手机屏幕尺寸来做适配。但如上所说，安卓手机的屏幕尺寸碎片化严重，这就需要我们写很多的媒体查询也不一定能覆盖所有设备全适配也不利于以后的扩展。因此，我们采用JS实现全适配，单位采用rem。
 我们的UI设计师给到的设计稿是iPhone6的尺寸，宽度是750px，那么我们前端在撸码的时候要把px换成rem，它们之间的换算关系如下：

```js
  	
	(function(doc, win) {
		var docEl = doc.documentElement, //html根元素
		resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
		recalc = function() {
			var clientWidth = docEl.clientWidth>1500?1500:docEl.clientWidth;//可视区域的宽度，iphone6下是375px
			if (!clientWidth) return;
			docEl.style.fontSize = 20 * (clientWidth / 750) + 'px';//在750px的设计稿中，1rem=10px，便于换算。
		};

	if (!doc.addEventListener) return;
	win.addEventListener(resizeEvt, recalc, false);
	doc.addEventListener('DOMContentLoaded', recalc, false);
	
	})(document, window);

```

### 2.通过zoom来设置：

作为一个早期IE的私有属性，其实现在的大部分浏览器也都能支持，zoom具有继承性，所以我们在最外层设置zoom，其子元素也会继承。

```js
          //获取做外层的元素
    	  var wrapper =document.getElementById("wrapper"); 
          //设置zoom
		     wrapper.style.zoom = Math.min(document.body.clientWidth, document.body.clientHeight) / 320;  //320是iphone4s的屏幕大小
```

所以当用zoom来做适配的时候，就可以完全采用px作单位来编写代码。



那在HTML中又需要设置哪些呢？

不同于以往的PC端页面开发，移动端页面的头部的meta中，下面这段话是必须配置的：
```

	<meta name="viewport" content="width=device-width, initial-scale=1.0,minimum-scale=1.0, maximum-scale=1.0, user-scalable=0">


```

content属性值 :

     width:网页可视区域的宽度，值可为数字或关键词device-width，可理解为网页的宽度等于手机屏幕的宽度

     height:同width

     intial-scale:页面首次被显示是可视区域的缩放级别，取值1.0则页面按实际尺寸显示，无任何缩放

     maximum-scale=1.0, minimum-scale=1.0;可视区域的缩放级别，

     maximum-scale用户可将页面放大的最大倍数，1.0将禁止用户放大到实际尺寸之上。

     user-scalable:是否可对页面进行缩放，no 禁止缩放


## 三.部分css样式重置
---
<font color="#5c969d">移动端绝大多数浏览器是WebKit内核，所以需加浏览器私有前缀-webkit-</font>
### 1.去除一些组件的默认样式
```css
input,
button,
textarea,
select,
optgroup,
option {
  -webkit-appearance: none;
}
```
### 2.输入框等组件点按会出现一个“暗色”的背景，去掉该背景的方法如下：

```css
a,button,input,optgroup,select,textarea {
    -webkit-tap-highlight-color:rgba(0,0,0,0); /*去掉a、input和button点击时的蓝色外边框和灰色半透明背景*/
}
```
### 3.禁用长按页面时的弹出菜单(iOS下有效) ,img和a标签都要加,方法如下:
```css
a, img {
    -webkit-touch-callout: none; /*禁止长按链接与图片弹出菜单*/
}
```
### 4.如何移除 input type="number" 时浏览器自带的上下箭头（以前项目中在小米手机中遇到过这个问题）

```css
input::-webkit-inner-spin-button {
-webkit-appearance: none;
}
input::-webkit-outer-spin-button {
-webkit-appearance: none;
}
```
### 5.禁止页面文字选择 
```css
body{
-webkit-user-select: none; 
}
```

## 四. 给body标签加上ontouchstart
---
<font color="#5c969d">还有就是在body标签内，最好也加上ontouchstart </font>, 因为在有的浏览器里面（safari），当你给一个类加上:active状态的时候，如果没有给body加上ontouchstart，那么active的状态就不出来。

## 五. 1px border问题
---

什么是1px border问题？

```
在 2014 年的 WWDC，“设计响应的 Web 体验” 一讲中，Ted O’Connor 讲到关于“retina hairlines”（retina 极细的线）：在 retina 屏上仅仅显示 1 物理像素的边框。
```

现在大家所用的手机大都是高清屏（Retina屏幕），其设备像素比为2或3或4较多，这就造成了在PC端网页中 1px 边框在移动端网页看起来有2px宽度的大小，在视觉体验上不友好，如下图：


![alt text](https://raw.githubusercontent.com/rancui/images/master/border.png "title")

这个问题也是一直存在但也是一些前端人员没有注意过的问题。至少在我所面试的大概10多个候选人中，只有两三个人听说过或者处理过这个问题（细节之一啊...）

![alt text](https://img.alicdn.com/tps/TB1uWfJIpXXXXaoXXXXXXXXXXXX.gif "title")
  
在普通屏幕下，1个css像素 对应 1个物理像素(1:1)。 在retina 屏幕下，1个css像素对应 4个物理像素(1:4)。



解决这个问题其实可以有以下6种方法：

#### 1. iOS8 以上支持 0.5px
实现原理：常规属性。

```css
.border {
    border: 0.5px solid #ff8100;
}
```

优点：原生、简单、常规写法。

缺点：目前只有 iOS8 以上系统才能支持，iOS7及以下、安卓系统都显示为 0px，可以通过脚本判断系统然后区分处理。

#### 2. CSS 渐变模拟
实现原理：设置 1px 通过 css 实现的背景图片，50%有颜色，50%透明，以上边框为例(结合媒体查询，最小设备像素比2)：

```css
@media only screen and (-webkit-min-device-pixel-ratio: 2) {
  .ui-border-t {
    border:0;
    background-position: left top;
    background-image: -webkit-gradient(linear, left bottom, left top, color-stop(0.5, transparent), color-stop(0.5, #cecece), to(#cecece));
    background-repeat: repeat-x;
    -webkit-background-size: 100% 1px;
 }
}
```

优点：兼容性较好，单边框、多边框可实现，大小、颜色可配置。

缺点：代码量多、同时占用了背景样式

#### 3. 阴影
实现原理：利用 css 对阴影处理的方式模拟。

```css
.border {
    -webkit-box-shadow: 0 1px 1px -1px rgba(0, 0, 0, 0.5);
}
```

优点：兼容性较好，单边框、多边框、圆角可实现，大小、颜色、可配置。

缺点：模拟效果强差人意，颜色不好配置。

#### 4. viewport + rem
实现原理：通过设置页面 viewport 与对应 rem 基准值。


```html
<!-- devicePixelRatio = 2：-->
<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">

<!-- devicePixelRatio = 3：-->
<meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no">
```

优点：兼容比较好，写法跟常规写法无异。

缺点：老项目改用 rem 单位成本较高。

#### 5. border-image
实现原理：通过图片配合边框背景模拟。

```css
.border-image{
    border-image: url() 2 0 stretch;
    border-width: 0 0 1px;
}
```

优点：无。

缺点：图片边缘模糊，大小、颜色更改不灵活。

#### 6. CSS3 缩放
实现原理：利用 :before/:after 重做 border，配合 scale 使得伪元素缩小一半

```scss
$bor_style : 1px solid #ff8100;
%border-top-1pt {
    content: '';
    height: 0;
    display: block;
    border-bottom: $bor_style;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
}
@media only screen and (-webkit-min-device-pixel-ratio:2) {
    %border-top-1pt {
        -webkit-transform: scaleY(0.5);
        -webkit-transform-origin: 50% 0%;
    }
}
```

优点：实现简单、单边框、多边框、圆角可实现，大小、颜色可配置。

缺点：代码量多，可通过 sass 预处理器处理。

#### 总结：
经过比较与实操测试，最好的处理方式是背景渐变和 CSS3 缩放，目前已经在项目中使用。



## 六.某些安卓机器，在低版本系统里遇到的键盘弹出挡住输入框问题
---

这个问题，在一些安卓机器的微信里面会出现，这和微信之前内置的X5内核浏览器不无关系。我们团队曾向腾讯负责X5内核浏览器的人反馈过，也得到了回应，给了一个二维码，下载了一个TBS工具集后，点击安装“TBS内核安装” 解决了这个问题。现在微信内置的已是Blink内核。

除此之外，也可以代码解决：
```js

	$(function(){
      var timer,windowInnerHeight2;
      var windowInnerHeight = document.documentElement.clientHeight;
      function eventCheck(e) {
          if (e) { //blur,focus事件触发的
              if (e.type == 'click') {//如果是点击事件启动计时器监控是否点击了键盘上的隐藏键盘按钮，没有点击这个按钮的事件可用，keydown中也获取不到keyCode值
              	
                  setTimeout(function () {//由于键盘弹出是有动画效果的，要获取完全弹出的窗口高度，使用了计时器
                      windowInnerHeight2 = document.documentElement.clientHeight;//获取弹出软键盘后的窗口高度
                  // alert(windowInnerHeight2)
                      $("html,body").scrollTop(windowInnerHeight-windowInnerHeight2-100);//此处数值可根据需要改变
                     timer = setInterval(function () { eventCheck() }, 50);
                  }, 500);
               
              }else{ 
            	  clearInterval(timer)
              };
          }else { //计时器执行的，需要判断窗口可视高度，如果改变说  明键盘隐藏了
              if (window.innerHeight > windowInnerHeight){
                  clearInterval(timer);
                 
              }
          }
      }
    
    $("xxx").on("click",eventCheck);// xxx代表你需要做处理的输入框，可自行替换成class或id
    
	})

```

## 七.加链接的快速反应

在我们做过的项目中推广活动页或多或少都有页面之间的链接，常用的是<a href="xxxxx"></a>， 但采用ontouchstart响应速度更快：
 

```html
<button class="ui-btn" ontouchstart="location.href='./xxx.html'"></button>
```



感谢您的阅读，以上就是在实际开发中的其中一点积累，希望能够给大家一些帮助。行文匆忙，如有错误不当之处还请指正。

如需转载，请注明出处~~









