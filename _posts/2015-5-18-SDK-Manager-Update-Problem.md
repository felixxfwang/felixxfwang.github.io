---
layout: post
category: Android
title: 解决墙内Android SDK无法更新
tags:
  - Android
  - SDK Manager
---

Google被墙之后，好长一段时间都是用VPN来更新Android SDK的，后来VPN到期了，没办法就只能寻找其他方法来更新SDK，下面是我总结的两种方法。

## 方法一：设置SDK Mananger代理

#### Android SDK在线更新镜像服务器

1. 中国科学院开源协会镜像站地址:  
IPV4/IPV6: http://mirrors.opencas.cn 端口：80  
IPV4/IPV6: http://mirrors.opencas.org 端口：80  
IPV4/IPV6: http://mirrors.opencas.ac.cn 端口：80  

2. 上海GDG镜像服务器地址:  
http://sdk.gdgshanghai.com 端口：8000

3. 北京化工大学镜像服务器地址:  
IPv4: http://ubuntu.buct.edu.cn/ 端口：80  
IPv4: http://ubuntu.buct.cn/ 端口：80  
IPv6: http://ubuntu.buct6.edu.cn/ 端口：80  

4. 大连东软信息学院镜像服务器地址:  
http://mirrors.neusoft.edu.cn 端口：80

5. 除了以上四个镜像服务器外，我还无意中发现了一个公益代理——红杏：  
 http://hx.gy 端口：1080

#### 设置SDK Manager代理

SDK Manager -> Tools -> Options，会出现下面的界面：  

![]({{ site.baseurl }}/img/sdk-manager-proxy-settings.png)  

按上图中红框标示区域设置Http Proxy Server和Port，并且勾选Force https://... sources to be fetched using http://...

修改完 reload即可。

## 方法二：修改hosts文件

在C:\Windows\System32\drivers\etc\hosts文件中加入以下几行：

    127.0.0.1 localhost

	#Google主页
	203.208.46.146 www.google.com

	#这行是为了方便打开Android开发官网 现在好像不VPN也可以打开
	74.125.113.121 developer.android.com

	#更新的内容从以下地址下载
	203.208.46.146 dl.google.com
	203.208.46.146 dl-ssl.google.com
	****************************************

其中最后两行是最重要的，其他都是可选。

## 总结

经本人亲测（也可能与我本身网络环境有关），方法二可行，但是速度较慢，推荐大家用方法一，尤其推荐用中国科学院开源协会镜像站的地址做代理，速度飞快。
