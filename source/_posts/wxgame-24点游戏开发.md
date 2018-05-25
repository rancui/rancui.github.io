---
title:  微信小程序初探之24点游戏开发
subtitle: 微信小程序初探之24点游戏开发
date: 2016-10-12 10:20:00
thumbnail: https://raw.githubusercontent.com/gbyuxia/myExercise/master/banner1.jpg
categories: Web开发
author:
  nick: 薛
  github_name: gbyuxia

---

小程序目前还在内测，不过微信已经发布了正式版开发者工具以及API，无需内测邀请也可以先试试了。这几天边学习边试着写了一个简单小游戏，做一个小分享，抛砖引玉，如有错误欢迎指正。

## 第一步：开发工具

[官方下载地址](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html)   选自己系统对应的版本下载，安装，微信扫码登录即可。

## 第二步：新建项目 ##

初学者可以从官网下载一些demo进行观摩学习，或者新建项目。本例从新建项目开始：

![](http://i.imgur.com/BlnPboo.png)

勾选在当前目录创建 quick start 项目会自动生成基础框架。创建成功后进入主窗口，全中文界面，非常清晰友好，无需多言。

![](http://i.imgur.com/771xRfZ.png)

我们先打开“编辑”界面，能看到除了page和utils两文件夹以外的app.js/app.wxss/app.json 三个文件，他们定义了小程序的框架。
其中，app.wxss 定义整个小程序的通用样式，我们可以理解成之前开发项目时常用的base.css 之类；之后的每个框架界面都会引用到这里的样式设置，并且如果框架界面里有同名的样式，将会覆盖它。

app.js里，App() 函数用来注册一个小程序。接受一个 object 参数，指定小程序的生命周期函数等；下面有一个 globalData 可以设置全局变量，我们设置一个最大数值：

    globalData:{maxNum:10}
       

在里面的框架页面如果需要用到此全局变量，代码如下：

    var appInstance = getApp();
	var max = appInstance.globalData.maxNum;

app.json文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等。我们先打开app.json文件，改动window下的设置，将标题栏文字改成项目名称，并对颜色做些设置。

开发阶段还可将**debug属性设置为true**，有助于调试。改完后的app.json整体如下：



    {
    "pages": [
        "pages/index/index",
        "pages/logs/logs"
    ],

    "window": {
        "backgroundTextStyle": "dark",
        "navigationBarBackgroundColor": "#06254c",
        "navigationBarTitleText": "24点游戏",
        "windowBackground": "#06254c",
        "navigationBarTextStyle": "white"
    },
    "debug":true
	}



之后我们打开pages/index文件夹，开始第一个页面的开发。

## 第三步：第一个界面

在我设想里，第一个界面将会有一个进入游戏的欢迎词、一个游戏玩法说明以及一个开始游戏的按钮。


因此在index.wxml里，我写上

    <view class="container">
		<view  bindtap="bindViewTap" class="userinfo">    
    		<text class="welcome">欢迎{{userInfo.nickName}}进入24点小游戏</text>
		</view>
		<view class="specification-box">
    		<text>{{specification}}</text>
		</view>
		<view class="page-bottom">
        	<button class="page-btn" bindtap="gotoCount">现在就开始玩吧</button>
    	</view>
	</view>

说明：[小程序里基础组件的API地址](https://mp.weixin.qq.com/debug/wxadoc/dev/component/?t=1476197491923)

然后在index.wxss里配置样式，基本上是css的简化版，前端都懂得。需要留意的是小程序给了两种单位：rpx和rem，都实现了屏幕适配，我们不需要额外纠结，看看[说明文档](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxss.html?t=1476197490824) 吧。

按钮上通过bindtap关联的gotoCount、通过api接口读取到的用户数据以及玩法说明等需要在index.js里定义：

	var app = getApp()    
	Page({
		data: {
	    	specification: '玩法说明：\n 进入游戏后，点击数字及运算符得出结果；前几步计算出的结果同样可以点击进行下一步运算。将给出的牌数用尽并得出24即为胜出。',
	    	userInfo: {}
		}, 
		//开始游戏 跳转到下一页  			
		gotoCount() {     
		     wx.navigateTo({ url: '../count/count'});
		},
		onLoad: function () {
			 var that = this
			 //调用应用实例的方法获取全局数据
			 app.getUserInfo(function(userInfo){
			  //更新数据
			  that.setData({
			        userInfo:userInfo
			      })
			    })
			  }
			})
	   
先通过getApp()取到app实例，在page里通过data绑定动态数据，其中的值在页面里通过花括号直接引用，如specification，也可以在各种交互中通过setData进行增改，如本例中,初始设定为空对象的userInfo 即在后面被设置成默认用户昵称。

navigateTo假如需要携带参数跳转，在url后加问好+参数名=参数值传递；在接收页 通过options接收处理， [具体说明](https://mp.weixin.qq.com/debug/wxadoc/dev/component/navigator.html?t=1476197486816)


## 第四步：编写游戏界面 ##

### 新建框架界面
要新建框架界面首先要先新建一个文件夹、在文件夹里新建四种类型的同名文件，并在app.json的page里将文件注册进去才可正常编译。如图：

![](http://i.imgur.com/ixUGwp8.png)

app.json 里，pages数组增加一行：

    
	"pages":[
	    "pages/index/index",
	    "pages/logs/logs",
	    "pages/count/count"
	],


现在去点击调试，将出现界面：

![](http://i.imgur.com/nPFM75o.png)

点击按钮 进入一个上面有菜单栏、下面空白的界面，这就是有待我们开发的count了。

### 先看一下界面 （红色为截图标注说明）


![](http://i.imgur.com/H2DDsJ3.png)

结果反馈：

![](http://i.imgur.com/Z6urlTD.png)

## count.js

count.js 里的代码结构：

![](http://i.imgur.com/YT2stEP.png)

点调试，看看默认的动态数据情况：

![](http://i.imgur.com/PL1i7vL.png)

展开：

![](http://i.imgur.com/wjuweYx.png)

这些就是在count.js的data里设定的（有些直接指定初始值、有些通过creatUnit生成设置）。对以上数据有概念之后去看界面：


## count.wxm

在count.wxml里别写几个区域，我的代码结构如下：

![](http://i.imgur.com/4Yje8nF.png)

分别来看：

# page-button这个view里有如下代码：

	<button  		
		wx:for="{{numbers}}" 
        bindtap="usetoCount" 
        disabled="{{disabled[index]}}" 
		class="{{disabled[index]?'card disabled':'card active'}}" 
        data-index = "{{index}}" 
        data-num="{{item}}">
             {{item}}
        </button> 

用了button组件，通过 wx:for 定义了循环，数据来源是numbers，它是count.js里的data里的numbers数组，结果是numbers数组有几个数据这边就将出现几个按钮（本游戏4个）；

for列表循环里默认index表示索引、item表示对应内容，也就是说假设一个['+','-']数组用于循环，则index和item将分别循环出0、1 以及 "+"、"-"。

[wx:for相关API地址](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/list.html?t=1476197491923)

按钮都绑定tap事件关联usetoCount方法;默认可点击，点击后将数字列到下面算式列表中，同时自身变成不可用状态；

按钮的样式通过disabled动态值判断，可点状态是card active否则是card disabled；

通过data-传送2个值，一个是index传递数字索引（用于判定第几个数字已经用了后面不能再用这种意思） 一个是 num 传递本按钮的值，用于运算，这些都将在usetoCount里用到。


放运算符的view里只有一句话：

    <button class="operator" bindtap="useOperator" wx:for="{{['+','-','*','/']}}" data-operator="{{item}}">{{item}}</button>
    

同上，用for循环加减乘除四个字符列出四个运算符按钮。

放运算列表的view里也是一个循环，取数据countLine，里面放点击后的数字以及运算符、等号按钮以及当前列计算结果

    <view class="count-list" wx:for="{{countLine}}">        
                <text class="number">{{item.firstNum}}</text>
                <text class="number">{{item.operator}}</text>
                <text class="number">{{item.nextNum}}</text>
                <button 
                    wx:if="{{item.firstNum && item.operator && item.nextNum}}" 
                    disabled="{{result.length>index}}" 
                    class="{{result.length>index?'operator disabled':'operator'}}" 
                    bindtap="toCount">
                        =
                </button>
                <button 
                    wx:if="{{result[index]}}" 
                    bindtap="usetoCount" 
                    data-num = "{{result[index]}}" 
                    class="{{disabled[index+4]?'operator disabled':'operator'}}" 
                    disabled="{{disabled[index+4]}}" 
                    data-index = "{{index+4}}">
                        {{result[index]}}
                </button>
            </view>    

    
最后 反馈用了另一个组件modal
	
    <modal 
		    title="{{isSuccessed?'祝贺你，成功了！':'失败了'}}" 
		    confirm-text="下一题" 
		    cancel-text="重新算一次" 
		    hidden="{{modalHidden}}" 
		    bindconfirm="getNextUnit" 
		    bindcancel="reCount"    
		/>

标题通过isSuccessed判定，两个按钮分别用于下一题以及重算。其中,modalHidden 初始设定为真，也就是开始默认隐藏，等结果出来后才显示反馈结果。

完整代码实现有点啰嗦，持续优化中，附上git地址：[https://github.com/gbyuxia/wx-game-24](https://github.com/gbyuxia/wx-game-24)

如需转载，请注明出处~~








