---
layout: post
title: Ubuntu server版本忘记用户密码的解决办法
categories: [Ubuntu]
tags: [Password]
description: 好久没有登录VirtualBox中的Ubuntu虚拟机，今天登录了一下，发现自己竟然忘记了用户的密码，root用户的密码也忘了，于是上网找了几篇文章看了一下，终于顺利解决了
---
{% include JB/setup %}
# Ubuntu server版本忘记用户密码的解决办法

好久没有登录VirtualBox中的Ubuntu虚拟机，今天登录了一下，发现自己竟然忘记了用户的密码，root用户的密码也忘了，于是上网找了几篇文章看了一下，终于顺利解决了。我用的是Ubuntu-server-12.04版本，下面介绍一下解决方法。

##1 解决方法

<strong>1.1</strong> 启动虚拟机，进入到GRUB，用键盘“↑”或“↓”键选择第二行（<strong>Ubuntu, with Linux 3.3.0-23-generic (recovery mode)</strong>）

<strong>1.2</strong> 按键盘“e”键进入编辑页面（注意不是回车）
<img src="/img/blog/ubuntu_server_grub.png" width="656px" height="563px" class="pic" alt="Ubuntu server版本忘记用户密码的解决办法" />

<strong>1.3</strong> 把<strong>ro recovery nomodeset</strong>改成<strong>rw single init=/bin/bash</strong>

<strong>1.4</strong> 按键盘<strong>Ctrl</strong>+<strong>x</strong>两个键，进入单用户模式  
<img src="/img/blog/ubuntu_server_grub_edit.png" width="656px" height="563px" class="pic" alt="Ubuntu server版本忘记用户密码的解决办法" />

<strong>1.5</strong> 修改用户的密码

{% highlight sh %}
cat /etc/shadow     # 查看当前用户（如果知道用户名，可以忽略）
passwd "username"   # 用户名为username，引号记得加上
{% endhighlight %}
然后提示输入用户密码，输入两边即可。

重启后即可登录。

##2 修改root用户密码

用户登录后，执行以下命令：

{% highlight sh %}
sudo passwd root # 获得root权限进行修改
# 输入两次密码，修改完成
{% endhighlight %}

<br />

##参考
[http://blog.csdn.net/zonelan/article/details/7968193](http://blog.csdn.net/zonelan/article/details/7968193)

[http://ljhzzyx.blog.163.com/blog/static/383803122010430114655408/](http://ljhzzyx.blog.163.com/blog/static/383803122010430114655408/)