---
layout: post
title: Ubuntu系统中安装、升级git以及常见的HTTPS cloning errors
categories: [Github]
tags: [Ubuntu, git, install, upgrade, clone, error]
description: 对刚刚接触Github的人们来说，安装及使用git是必须学会的，不过本人在Ubuntu系统安装git的时候也出现多几个问题，现在总结记录以下，也从网上找了一些常见的问题，方便以后使用的时候参考参考，免得下次安装再出现类似问题
---
{% include JB/setup %}

#Ubuntu系统中安装、升级git以及常见的HTTPS cloning errors

对刚刚接触Github的人们来说，安装及使用git是必须学会的，不过本人在Ubuntu系统安装git的时候也出现多几个问题，现在总结记录以下，也从网上找了一些常见的问题，方便以后使用的时候参考参考，免得下次安装再出现类似问题。

##1 Windows、MAC下安装git

首先简单介绍以下其他系统中如何安装git。

1） 在Windows系统中，安装git相当简单，直接下载git的应用程序，像普通应用程序安装一样，下载地址点击[这里](http://github-windows.s3.amazonaws.com/GitHubSetup.exe)，安装好之后，会有一个图形界面，使用很方便，也可以使用命令来创建项目，因为它也帮你安装了一个Git Shell。

2） 在MAC系统中，由于本人还未使用过，可以参考一下这篇文章[Mac OS X安装Git](http://blog.csdn.net/yhawaii/article/details/7519440)。不过github官网的建议是：使用本地app代替使用终端安装，下载APP链接为：[https://central.github.com/mac/latest](https://central.github.com/mac/latest)。

##2 Ubuntu下安装、升级git

介绍一下个人觉得最方便，最好的方法，那就是使用添加ppa的方法来安装。

ppa的地址：[https://launchpad.net/~git-core/+archive/ppa](https://launchpad.net/~git-core/+archive/ppa)

在终端运行以下命令

<pre class="command-line">
    <span class="command">$ sudo apt-add-repository ppa:git-core/ppa</span>
    <span class="command">$ sudo apt-get update</span>
    <span class="command">$ sudo apt-get install git</span>
</pre>

如果本地已经安装过git，可以使用升级命令：

<pre class="command-line">
    <span class="command">$ sudo sudo apt-get dist-upgrade</span>
</pre>

##3 运行git clone HTTPS出现的错误及解决办法
在使用<code class="cd">git clone</code>命令时，使用https链接进行clone时，可能会出现以下[错误](https://help.github.com/articles/https-cloning-errors)：

<pre class="command-line">
    <span class="output">error: The requested URL returned error: 401 while accessing</span>
    <span class="output">https://github.com/<em>user</em>/<em>repo</em>.git/info/refs?service=git-receive-pack</span>
    <span class="output">fatal: HTTP request failed</span>
</pre> 

<pre class="command-line">
    <span class="output">Error: The requested URL returned error: 403 while accessing</span>
    <span class="output">https://github.com/<em>user</em>/<em>repo</em>.git/info/refs</span>
    <span class="output">fatal: HTTP request failed</span>
</pre> 

<pre class="command-line">
    <span class="output">Error: https://github.com/<em>user</em>/<em>repo</em>.git/info/refs not found: did you run git</span>
    <span class="output">update-server-info on the server?</span>
</pre> 

如果出现上述错误，解决办法如下：

1） 检查以下你的git版本，如果你的版本低于1.7.10，那么你就需要升级以下你的git，升级方法在上面已经介绍里，命令：

<pre class="command-line">
    <span class="command">$ git --version</span>
    <span class="output">git version <em>1.7.10.4</em></span>
</pre> 


2） 检查以下你的远程仓库是否存在于Github上，并且URL是大小写敏感的，看看是不是字字母大小写弄错了。比如我的运行程序：

<pre class="command-line">
    <span class="command">$ git remote -v</span>
    <span class="comment"># View existing remotes</span>
    <span class="output">origin git@github.com:lym406365619/lym406365619.github.com.git (fetch)</span>
    <span class="output">origin git@github.com:lym406365619/lym406365619.github.com.git (push)</span>
</pre>

如果是在不行的话，使用SSH代替HTTPS，这个好像安装之后就可以用，不会出错。向Github添加SSH keys的教程可以参考以下我的这篇文章，点击[这里](http://lym406365619.github.io/blog/2013/05/16/add-ssh-keys-into-your-github)。
