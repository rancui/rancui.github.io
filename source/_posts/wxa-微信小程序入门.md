---
title: 微信小程序入门
subtitle: 实在不知道起什么标题，然后想想，这是基础，就起了这个XX标题～😄～～
thumbnail: https://raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-012.jpg
categories: Web开发
tags: 微信小程序
author:
  nick: ofroad
  github_name: ofroad
date: 2016-09-29 23:14:33
---
9月22日，微信小程序开始小范围内测。几天后微信官方发布了小程序开发文档以及小程序开发者工具。
一口气看完了所有的开发文档，一个很直观的感受就是前端可以直接使用前端知识以及借助后端云服务很方便的开发应用了。

下面就讲一下微信小程序相关的基本用法

### 安装小程序开发者工具

从微信官方[下载小程序开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html)并安装，安装好后启动开发者工具，使用微信扫描登录。
![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-001.png)

### 新建项目

![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-002.png)
点击“添加项目”，填写AppID(如果没有AppID，选择下方的无AppID即可)和项目名称，选择项目目录，点击添加项目即可。
![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-003.png)
初次使用可以选择下面的创建一个 quick start 项目，这样会在开发目录里生成一个简单的 demo。
![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-004.png)

### 小程序开发者工具介绍

(1)编辑
![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-006.png)
编辑界面左边为项目的文件列表，右边为编辑窗口。在文件列表中点击文件即可在编辑窗口中对此文件进行编辑。

(2)调试
![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-011.png)
调试界面左边页面预览，其中左边的上面可以选择模拟的机型和网络
右边和chrome的调试工具界面非常类似。其中Storage是应用在本地存储的数据，AppDate是当前页面的一些数据信息。

(3)项目
![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-008.png)
此界面为应用上传和预览界面

(4)重启
重启当前应用

(5)后台
![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-009.png)
当前应用在后台运行，可以再次切换到前台

(6)缓存
可以清除本地存储的数据和文件

(7)关闭
关闭当前应用，回到登录(启动)后的界面

### 小程序代码结构介绍

![](//raw.githubusercontent.com/ofroad/git-learn/master/wxa/wxa-010.png)
app.js
app.js主要定义全局的方法和变量以及监听并处理小程序的生命周期

app.json
app.json是应用的全局配置文件，配置应用的所有组成页面和应用的界面样式

app.wxss
app.wxss是应用的公共样式文件，和css基本一致

pages目录
pages目录下面是应用的所有页面，每个页面都可以单独定义一个目录，目录里面包含4个文件。
wxml文件是当前页面的结构文件
js文件是当前页面的脚步文件
wxss文件是当前页面的样式文件，与公共样式文件app.wxss叠加使用
json文件是当前页面的配置文件，与全局配置文件app.json叠加使用

### 其它
小程序结构这块，微信官方定义了一套框架，了解过vue的就会感觉似曾相识。
样式这块，微信做了一些自己的扩展和限制，详见[WXSS](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxss.html)



以上就是微信小程序的基本介绍。更详细的情况请参考[微信官方开发文档](https://mp.weixin.qq.com/debug/wxadoc/dev/)

如需转载，请注明出处~~