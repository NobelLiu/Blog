---
title: 调整好你的网络
date: 2016-05-29 20:34:28
category: Other
tags:
- Shadowsocks
- Proxy
- OS X
---
{% cq %} 
缅甸最近宣布对全球最受欢迎的社交网站Facebook解禁，目前全球仅剩4个国家仍然对Facebook实施封锁，他们分别是朝鲜、古巴、伊朗，和其他国家。 
{% endcq %}

当时看到这个新闻真算是把我逗得捧腹了，在大陆，由于[防火墙](https://zh.wikipedia.org/wiki/%E9%98%B2%E7%81%AB%E9%95%BF%E5%9F%8E)的存在，导致我们在访问很大部分的国外知名网络服务时，一般看到的都是这样的景象:
<!-- more --> 
![无法访问](https://ww1.sinaimg.cn/large/006y8lVagw1fbljnkiy23j31kw17qgs6.jpg)  
从 2012 年起往后 Google 全面被封锁到“不能用”，到百度的广告越来越猖獗到“不能用”，老百姓想好好查个资料都不行。
好吧，那就说下我的翻墙手段吧。  
本文默认如下操作环境：  
Macbook Pro with Retina Display 2015 ＋ OS X 10.11.5  
如有不同，聪明的你一定会找到相对应的解决办法 😉

# 连接“互联网”
## Shadowsocks
**多平台 速度快 配置简单**是 Shadowsocks 的优点。Shadowsocks 本身是免费开源项目，但服务器通常不是免费的，虽说有免费的服务器，但性能通畅很糟糕。作为程序员来说，糟糕的网络直接带来糟糕的心情，可以有效降低你的工作效率 😂  
 
Shadowsocks 的服务器有两种：别人搭建好的和自己搭好的，身为程序员当然是要自己动手搭最安全最便宜最爽啦～   
![Vultr 首页](https://ww2.sinaimg.cn/large/006y8lVagw1fbljo6vv1nj31kw0q8agc.jpg)

服务器我采用的 Vultr 的服务器，目前用起来一切都好，点击[本链接](http://www.vultr.com/?ref=6837582)注册，可获得10元优惠券，建好账号绑好支付方式10刀就已经在你账户啦。  

不同价位机器可以根据自身情况选择，单人可以最低配，多人自行向上上升档次，关于操作系统我选择了 [Cent OS 7](https://www.centos.org/)。    

按照指引启动好机器，可以看[《CentOS下shadowsocks-libev一键安装脚本》](https://teddysun.com/357.html)来快速安装 Shadowsocks，有兴趣也可以装下 VPN，文章在此[《PPTP一键安装脚本》](https://teddysun.com/134.html)。  

接下来可以在[shadowsocks.org](https://shadowsocks.org/en/download/clients.html)下载你的客户端。  

OK 到此为止，假定你已经部署好服务器也下好了客户端并且连上了 Shadowsocks，你也会发现 Google Twitter Github 也都可以流畅的访问，但是这一切都还没有完。你用了一段时间，你就会发现，你下载的部分 App 以及你的终端都不能很好的“翻墙”，这是由软件本身是否支持 socks 的代理方式决定的，对于不兼容的 App，该怎么办呢？  
  
## Proxifier or Proximac
{% cq %} 
Proxifier 是一款功能非常强大的socks5客户端，可以让不支持通过代理服务器工作的网络程序能通过HTTPS或SOCKS代理或代理链。支持64位系统，支持 Xp,Vista,Win7,支持socks4,socks5,http代理协议，支持TCP,UDP协议，可以指定端口，指定IP,指定程序等运行模式，兼容性非常好。
　　有许多网络应用程序不支持通过代理服务器工作，因此不能用于局域网或防火墙后面。这些会损害公司的隐私和导致很多限制。Proxifier 解决了这些问题和所有限制，让您有机会不受任何限制使用你喜爱的软件。此外，它让你获得了额外的网络安全控制，创建代理隧道，并添加使用更多网络功能的权力。
{% endcq %}  


![Proxifier 截图](https://ww2.sinaimg.cn/large/006y8lVagw1fbljotzifjj31a81feqmy.jpg)

[Proxifier](https://www.proxifier.com/) 是**收费**软件，包含 GUI，如果你不想付费或其他原因，可以使用Proxifier的**免费开源**版本 [Proximac](https://github.com/csujedihy/proximac)。    
 
![Proximac 截图](https://ww1.sinaimg.cn/large/006y8lVagw1fbljpkxgvyj312q0qk7as.jpg)
两者操作都算简单，可以参照官网或查阅相关资料。
# 使用“互联网”
至现在为止，你应该可以比较轻松的访问大部分被封锁的网络服务，那么哪些是值得我们去用的呢？
可以说：太多了...  
多多 Google 一下，看看有哪些好产品 好服务，快快丢掉你手中广告满满，速度慢慢的产品吧。