---
layout: post
title: 购买域名后如何绑定在Github上托管的网站
categories: [Github]
tags: [域名, 绑定, CNAME, DNS, A记录, GoDaddy]
description: 在介绍之前，首先得搞清楚Github托管项目所支持的类型，一种是User/Organization Pages，另一种是Project Pages，这是两种Github托管网站上支持的两种基本类型.这个是Github为每个账户分配的一个二级域名，不过需要注意的是
---
{% include JB/setup %}
#购买域名后如何绑定在Github上托管的网站

##1 前言

在介绍之前，首先得搞清楚Github托管项目所支持的类型，一种是<code class="cd">User/Organization Pages</code>，另一种是<code class="cd">Project Pages</code>，这是两种Github托管网站上支持的两种基本类型。

###User/Organization Pages
这个是Github为每个账户分配的一个二级域名，不过需要注意的是必须以那你的用户名作为开头,不然无法访问。例如我的用户名为lym406365619。如果我想使用Github给我分配的二级域名，必须创建名字为<code class="cd">lym406365619.github.io</code>的仓库。

###Project Pages
除了上述提到以自己的名字来命名的仓库，其他所建的都可以看成是Project Pages。不过要想实现对页面的访问，则就需要创建<code class="cd">gh-pages</code>分支来访问。

##2 在版本库根目录创建名为CNAME文件
在版本版本库根目录下创建名为CNAME文件，并且将你的域名或者二级域名写在文件中。例如我的购买的域名为[http://luyueming.info](http://luyueming.info)，想通过这个域名能直接访问到[http://lym406365619.github.io](http://lym406365619.github.io)中的内容，则我需要在这个版本库的根目录下创建名为<code class="cd">CNAME</code>的文件<code class="cd">（注意大小写）</code>里面的内容为购买域名的网址：
{% highlight yaml %}
luyueming.info
{% endhighlight %}
如果你使用的是User/Organization Pages版本库，需要在<code class="cd">master</code>分支根目录下创建该文件；如果使用Project Pages版本库，则需要在<code class="cd">gh-pages</code>分支根目录下创建该文件。

创建好之后，需要等待10分钟左右Github才会生效，可以在这段时间进行下一步操作。

##3 设置DNS
进入你的域名管理，我的域名是在[GoDaddy](http://www.godaddy.com/)购买的，主要国外的比较便宜，而且不用备案，很方便呀。登录官网后，进入域名管理界面，里面有个<code class="cd">DNS Manager</code>，点击<code class="cd">Launch</code>，进入如下页面：

<img src="/img/blog/host.jpg" width="750px" height="470px" alt="GoDaddy设置DNS，绑定域名" class="pic" />

如果你想绑定在你的<code class="cd">主域名</code>中，你必须在<code class="cd">A记录</code>下添加一条记录指向IP<code class="cd">204.232.175.78</code>。如图所述，我在A(Host)下只有一条记录，Host为@，代表网址为luyueming.info。Points to则代表你所指向的IP，这里我们是204.232.175.78。编辑好之后点击图片右上角保存就行。

如果你想绑定在<code class="cd">子域名</code>中，你最好创建一条<code class="cd">CNAME</code>记录指向你的二级域名。在CNAME下点击Quick Add就行，Host可以写一个你喜欢的名称，比如我写成blog，则代表网址blog.luyueming.info，Points to指向你在Github上托管访问的网址。点击保存即可。

##4 结语
本人自己测试了一下自己的网站，终于将自己购买的域名绑定好了。之前也看了挺多资料，就是不知道怎么弄，后来发现原来不是很难。于是写下来作为参考，如果疑问请留言！