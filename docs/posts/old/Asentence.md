---
layout: post
title: "Asentence"
tagline: "A platform for comment in one sentence"
description: "A platform for comment in one sentence"
category: projects
tags: [android,webkit,tornado,mysql,phonegap]
excerpt: "Asentence is platform for users to publish topics and comment on them. All of the statements should be only 1 sentence. It's fun on some platforms like Weibo."
---
{% include JB/setup %}

## 产品背景
   人们渐渐倾向于用简短的语言制造有趣的话题；
   处于信息爆炸时代，人们渴望在短时间了解到更多的信息；
   微博信息量较大且功能趋于复杂，用户体验下降。

<img class="img-responsive" style="height: 300px;" src="https://github.com/ghostbody/Asentence/blob/master/doc/view/icon.png?raw=true">


## 用户群体
  主要针对经常使用移动智能客户端的群体。

## 功能介绍

### 1 登录及注册

1. 登录：支持微信、微博、QQ绑定，一键登录；
2. 注册：邮箱注册，用户自定义密码。

### 2 首页——所有话题

1. 向用户推送精彩话题，话题范围涵盖生活、时事、科技、书籍、电影等。
2. 话题内容简短有趣，符合大众兴趣，容易引起用户共鸣。
3. 用户可根据“热度”“时间”“推荐”对内容进行排序。

### 3 创建——我要推荐

1. 用户可发起新话题，需经后台管理员审核，审核通过则将在首页推出。
2. 用户可以在“我的推荐”页面查看发起的话题以及当前审核状态


<img class="img-responsive" style="height: 300px;" src="https://github.com/ghostbody/Asentence/blob/master/doc/view/1.%E9%A6%96%E9%A1%B5.jpg?raw=true">

<img class="img-responsive" style="height: 300px;" src="https://github.com/ghostbody/Asentence/blob/master/doc/view/2.%E9%A6%96%E9%A1%B5%E7%AD%9B%E9%80%89.jpg?raw=true">


### 4 收藏——我的分类

1. 在首页点击左上角用户头像，可以查看我的分类，点击相应标签，可以查看对应已收藏内容；
2. 用户可以自定义分类标签及对已有标签进行修改；
3. 在“热门收藏”模块可以查看用户普遍感兴趣的话题和精彩评论

### 产品创新点

1. 对于任何话题，用户只能用一句话发表看法，且有数字要求。
2. 内容丰富，且可以由用户发起，满足用户各种需求。
3. 界面简洁，功能简单，在娱乐大众同时传递最新消息，方便用户快速吸收。

<img class="img-responsive" style="height: 300px;" src="https://github.com/ghostbody/Asentence/blob/master/doc/view/3.%E4%B8%AA%E4%BA%BA%E8%8F%9C%E5%8D%95.jpg?raw=true">

<img class="img-responsive" style="height: 300px;" src="https://github.com/ghostbody/Asentence/blob/master/doc/view/4.%E4%B8%AA%E4%BA%BA%E8%8F%9C%E5%8D%95%E6%96%B0%E5%BB%BA.jpg?raw=true">

##项目构架

服务端：Python Tornado + MySQL

客户端：安卓端 + Web端 + 电脑桌面应用

##开发者调试

    git clone https://github.com/ghostbody/Asentence

###服务端：

    cd ./Asentence/src/server
    python index.py

~~运行客户端：~~
~~使用Android Studio 打开项目 \$ASENTENCE$/src/client，然后编译运行~~

###安卓端：

html5+css+js+ratchet+phonegap

1. 安装sdk（安卓 api >= 19）配置好环境变量

2. 安装nodejs（以及npm）建议下载编译运行

3. 安装phonegap（或者cordova）


    sudo npm -g install phonegap
    sudo npm -g install cordova


4. 安装号gradle(2.2.1)

5. 连接好安卓手机(需要进行了root并且开启usb调试)，运行/src/android下的makefile文件

    make

## 桌面端

c++ webkit + html + css + js

开发中

## Github

[https://github.com/ghostbody/Asentence](https://github.com/ghostbody/Asentence)
