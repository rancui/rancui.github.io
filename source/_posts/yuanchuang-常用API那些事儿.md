title: JavaScript常用API那些事儿
subtitle: 常用的JavaScript的API
date: 2016-10-10
thumbnail: https://raw.githubusercontent.com/rancui/images/master/api.jpg
categories: Web开发
author:
  nick: 一念rancui
  github_name: rancui
  
tags:
  - 项目总结

---



短暂的十一假期，感觉还没开始就结束了。。。。

![](https://raw.githubusercontent.com/rancui/images/master/nu.jpg "title")

但是，说过的话话是要算数滴~~


## 今天我们就来聊聊JavaScript常用的API那些事儿吧~~~
---

想到js，整个人的脑袋就感到······

![](https://raw.githubusercontent.com/rancui/images/master/duang.jpg "title")


但从事前端开发，js是必须要会的。不仅仅是前端人员，后端人员也会用到。


那么，在工作中常用到javascript的API有哪些呢？

## 一. 元素查找
---
```js

    //node
	document.getElementById();// 这个就不用注释是啥意思了吧？！
	document.querySelector();//该方法返回文档中匹配指定 CSS 选择器的第一个元素。
	document.body;// 返回文档的body元素
	document.head;// 返回文档的head元素
	document.title;// 返回当前文档的标题
	document.documentElement;//根元素，指html
	document.doctype;// 返回 html文档的文档类型对象
	document.readyState;// 返回文档状态 (载入中……)
	
	//nodeList
	document.querySelectorAll();
	document.getElementsByClassName();
	document.getElementsByTagName();
    document.scripts; // 返回页面中所有脚本的集合
	document.forms; // 返回对文档中所有 Form 对象引用
	document.images;// 返回对文档中所有 Image 对象引用。
	
	

```


## 二. class操作
---

我们平时常用的jquery里面有**addClass(), removeClass(),toggleClass(),hasClass()**这些api。

但HTML5时代，classList API出现了，classList 属性返回元素的类名，作为 DOMTokenList 对象。

classList实际上已经出现好多年了，因此，FireFox浏览器，Chrome浏览器都支持。IE家族中，从IE10浏览器开始才开始认可classList。

移动端，Android 3.0+以上才开始支持，时至今日，移动端可以说已经几乎全部支持该api。


```js

     //增加类名, 以下皆以"classname"类名为例。
     document.getElementById("myDIV").classList.add("classname");

    //移动类名
    document.getElementById("myDIV").classList.remove("classname");

    //返回布尔值，判断指定的类名是否存在
    document.getElementById("myDIV").classList.contains("classname");

    //返回类名在元素中的索引值。index是索引值，索引值从 0 开始;
    document.getElementById("myDIV").classList.item(index);

    //类名切换
    document.getElementById("myDIV").classList.toggle("classname");

```



## 三. 属性操作
---

```js

    //元素的图片路径
    element.getAttribute("src");

    //设置元素的图片路径
    element.setAttribute("src","图片的路径");


```

## 四. 节点操作
---


```js

    //创建元素
    var element = document.createElement(name);

    // true，克隆节点及其属性和后代； false，克隆节点和后代；
    element.cloneNode(true);

    //追加子元素
    element.appendChild(child);

    //父元素
    element.parentNode;
 
    //删除子元素
    element.parentNode.removeChild(child);

    //在指定的节点前插入子节点
    parentNode.insertBefore(element,parent.childNodes[0]);



```

## 五. css操作
---
获取元素的样式一般有currentStyle，getComputedStyle,style这三种来就获取css的值，
jquery中的css()底层其实也是用到了getComputedStyle。

以下是获取css样式,以元素element为例。

```js
    
    element.currentStyle[attrName]；//注意，此处用的是元素element

    window.getComputedStyle(element)[attrName];//注意，此处用的是window

    window.getComputedStyle(element,":before");//注意，此处用的是window

    //在移动端我们用style设置css样式的时候一般采用cssText比较好,可以批量操作css样式。
    //cssText只需一次reflow，提高了页面渲染性能, 例如：

    element.style.cssText="width:100px;height:100px; -webkit-transform:translate3d(0,100%,0)";




```

## 六. 位置大小

在我们实际的工作中，经常会用位置方面的的知识点来获取或者设置一些元素。以下以元素element为例：


```js

    //对象可见内容的宽度(高度同理)不包括滚动条，不包括边框
    element.clientWidth;

    //当前对象的宽度(width+padding+border)
    element.offsetWidth;
 

    //获取对象的滚动宽度
    element.scrollWidth;

    //获取对象的滚动宽度
    element.scrollWidth;

    //获取对象的滚动宽度
    element.scrollWidth;

    //获取对象的滚动宽度
    element.scrollWidth;


    //相对整个页面的坐标
    element.pageY

  
    //相对可视区域的坐标
    element.clientY

    //鼠标在屏幕中的位置，指的是鼠标到电脑屏幕左侧的距离
    //与clientX的区别是clientX是到浏览器的距离。例如：当网页缩小，拖动到屏幕中间时，screnX 大于 clientX
    element.screenX 

    //设置或获取位于对象左边界和窗口中目前可见内容的最左端之间的距离，也就是元素被滚动条向左拉动的距离。
    element.scrollLeft；

    //设置或获取位于对象最顶端和窗口中可见内容的最顶端之间的距离，也就是元素滚动条被向下拉动的距离。
    element.scrollTop；




```

除以上外，还可以用**getBoundingClientRect**获取元素位置。

getBoundingClientRect用于获得页面中某个元素的左，上，右和下分别相对浏览器视窗的位置。该函数返回一个Object对象，该对象有6个属性：top,lef,right,bottom,width,height；这里的top、left和css中的理解很相似，width、height是元素自身的宽高，但是right，bottom和css中的理解有点不一样。right是指元素右边界距窗口最左边的距离，bottom是指元素下边界距窗口最上面的距离。

```js
  
	  getBoundingClientRect()
	
	    这个方法返回一个矩形对象，包含四个属性：left、top、right和bottom。分别表示元素各边与页面上边和左边的距离。
	
	 
	
	var box=document.getElementById('box');         
	
	console.log(box.getBoundingClientRect().top);        
	
	console.log(box.getBoundingClientRect().right);      
	
	console.log(box.getBoundingClientRect().bottom);     
	
	console.log(box.getBoundingClientRect().left);       
	
	 
	
	注意：IE、Firefox3+、Opera9.5、Chrome、Safari支持，在IE中，默认坐标从(2,2)开始计算，导致最终距离比其他浏览器多出两个像素，我们需要做个兼容。
	
	 
	
	document.documentElement.clientTop;  // 非IE为0，IE为2
	
	document.documentElement.clientLeft; // 非IE为0，IE为2
	
	functiongGetRectObj (element) {
	
	    var rect = element.getBoundingClientRect();
	
	    var top = document.documentElement.clientTop;
	
	    var left= document.documentElement.clientLeft;
	
	    return{
	
	        top    :   rect.top - top,
	
	        bottom :   rect.bottom - top,
	
	        left   :   rect.left - left,
	
	        right  :   rect.right - left
	
	    }
	
	}

```



## 七. 移动端事件
---


移动端没有鼠标事件，只有触摸事件，分别是ontouhstart，ontouchmove，ontouchend。

#### 1.在非IE的标准浏览器中，addEventListener()绑定事件的对象方法。

addEventListener()含有三个参数，一个是事件名称，另一个是事件执行的函数，最后一个是事件捕获。

obj.addEventListener("touchmove",function(){},true/false);

这里的事件名称跟直接写的事件名称不一样，在这里前面没有on。
#### 2.事件属性
无论使用触摸还是手势事件，你都需要将这些事件转换为单独的触摸来使用它们。为此，你需要访问事件对象的一系列的属性。

touches: 当前屏幕上所有触摸点的列表;

targetTouches: 当前对象上所有触摸点的列表;

changedTouches: 涉及当前事件的触摸点的列表


用一个手指接触屏幕，触发事件，此时这三个属性有相同的值。

用第二个手指接触屏幕，此时，touches有两个元素，每个手指触摸点为一个值。当两个手指触摸相同元素时，targetTouches和touches的值相同，否则targetTouches 只有一个值。changedTouches此时只有一个值，为第二个手指的触摸点。
用两个手指同时接触屏幕，此时changedTouches有两个值，每一个手指的触摸点都有一个值
手指滑动时，三个值都会发生变化
一个手指离开屏幕，touches和targetTouches中对应的元素会同时移除，而changedTouches仍然会存在元素。
手指都离开屏幕之后，touches和targetTouches中将不会再有值，changedTouches还会有一个值，此值为最后一个离开屏幕的手指的接触点。



#### 3.在IE浏览器中，attachEvent()绑定事件的对象方法。


obj.attachEvent("onclick",function(){});

注意此方法中的事件名称要加上on，没有事件捕获。



## 八. 创建文档碎片
---

如果当我们要向document中添加大量数据时(比如1w条)，因为每添加一个节点都会调用父节点的appendChild()方法，这个过程就可能会十分缓慢。因此我们可以引入createDocumentFragment()方法，它的作用是创建一个文档碎片，把要插入的新节点先附加在它上面，然后再一次性添加到document中，使用DocumentFragment进行缓存操作，引发一次回流和重绘，提高性能。

```js

	//先创建文档碎片
	
	var Fragmeng = document.createDocumentFragment(); 
	
	
	for(var i=0;i<10000;i++)
	
	{ 
	
	    var aSpan = document.createElement("span"); 
	
	    var aText = document.createTextNode(i); 
	
	    aSpan.appendChild(aText); 
	
	    //先附加在文档碎片中
	
	    Fragmeng.appendChild(aSpan);  
	
	} 
	
	//最后一次性添加到document中
	
	document.body.appendChild(Fragmeng); 

```



以上就是一些分享和总结，如需转载，请注明出处~~
































