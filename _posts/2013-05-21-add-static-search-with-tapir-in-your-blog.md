---
layout: post
title: 使用Tapir为Jekyll博客实现全局搜索
categories: [Jekyll]
tags: [Tapir, search, SEO, RSS]
description: 想在Jekyll博客上添加一个静态搜索，这样方便搜索相应的关键字。虽然自己也把博客归类，并给每篇博客贴上标签，但是个人还是比较喜欢折腾，总感觉有个搜索功能是非常好的一件事情。于是上网也搜集了一下相关资料，发现Jekyll博客添加搜索功能还是稍微有点麻烦的，而且有些功能还不是很好
---
{% include JB/setup %}

#使用Tapir为Jekyll博客实现全局搜索

想在Jekyll博客上添加一个静态搜索，这样方便搜索相应的关键字。虽然自己也把博客归类，并给每篇博客贴上标签，但是个人还是比较喜欢折腾，总感觉有个搜索功能是非常好的一件事情。于是上网也搜集了一下相关资料，发现Jekyll博客添加搜索功能还是稍微有点麻烦的，而且有些功能还不是很好。只能搜索英文关键字，兼容性也有点欠缺，有的也只是实现博客标题以及日期搜索。

个人大致归类了一下，在Jekyll博客搜索主要有一下几种实现：

1） 利用Plugin + JavaScriot的方式；

2） 利用Google提供的[Google Custom Search](http://www.google.com/cse/manage/all)方法，这个操作很简单，功能很好，但在国内来说不是很稳定，不太实用；

3） 实用JQuery搜素静态文本的API来解决。

下面我介绍一种非常简单的站内搜素——[Tapir](http://tapirgo.com/)。这也是我参考网上Dan's的一篇文章叫[《使用Tapir实现jekyll的站内搜索功能》](http://www.shanhh.com/blog/2012/11/16/tapir-search-for-jekyll/)。

从官网可以得知，Tapir是专门为静态页面，比如博客等设计出来的一种站内搜素功能。可以通过简单的RSS订阅创建索引，并返回JASON格式的搜素结果，并且对中文支持也非常棒。下面我介绍一下如何部署。

## 1 添加RSS feed
如果你的博客还没有创建RSS feed，请在你的根目录下创建一个名为atom.xml的文件，添加一下内容：

<div class="highlight">
 <pre>
   <code class="text">---
layout: nil
title : Atom Feed
---
<span class="nt">&lt;?xml</span> version="1.0" encoding="utf-8"<span class="nt">?></span>
<span class="nt">&lt;feed</span> xmlns="http://www.w3.org/2005/Atom"<span class="nt">></span>
 
  <span class="nt">&lt;title></span>&#123;{ site.title }}<span class="nt">&lt;/title></span>
  <span class="nt">&lt;link</span> href="&#123;{ site.production_url }}/&#123;{ site.JB.atom_path }}" rel="self"<span class="nt">/></span>
  <span class="nt">&lt;link</span> href="&#123;{ site.production_url }}"<span class="nt">/></span>
  <span class="nt">&lt;updated></span>&#123;{ site.time | date_to_xmlschema }}<span class="nt">&lt;/updated></span>
  <span class="nt">&lt;id></span>&#123;{ site.production_url }}<span class="nt">&lt;/id></span>
  <span class="nt">&lt;author></span>
    <span class="nt">&lt;name></span>&#123;{ site.author.name }}<span class="nt">&lt;/name></span>
    <span class="nt">&lt;email></span>&#123;{ site.author.email }}<span class="nt">&lt;/email></span>
  <span class="nt">&lt;/author></span>

  &#123;% <span class="jk">for</span> post in site.posts %}
  <span class="nt">&lt;entry></span>
    <span class="nt">&lt;title></span>&#123;{ post.title }}<span class="nt">&lt;/title></span>
    <span class="nt">&lt;link</span> href="&#123;{ site.production_url }}&#123;{ post.url }}"<span class="nt">/></span>
    <span class="nt">&lt;updated></span>&#123;{ post.date | date_to_xmlschema }}<span class="nt">&lt;/updated></span>
    <span class="nt">&lt;id></span>&#123;{ site.production_url }}&#123;{ post.id }}<span class="nt">&lt;/id></span>
    <span class="nt">&lt;content</span> type="html">&#123;{ post.content | xml_escape }}<span class="nt">&lt;/content></span>
    <span class="nt">&lt;summary</span> type="html">&#123;{ post.description | xml_escape }}<span class="nt">&lt;/summary></span>
  <span class="nt">&lt;/entry></span>
  &#123;% <span class="jk">endfor</span> %}

<span class="nt">&lt;/feed></span></code></pre>
</div>

之后在你的html页面head标签中添加下面代码：
<div class="highlight">
 <pre>
   <code class="text"><span class="nt">&lt;link</span> href="{{ BASE_PATH }}{{ site.JB.atom_path }}" type="application/atom+xml" rel="alternate" title="Sitewide ATOM Feed"<span class="nt">></span></code></pre>
</div>

若果你已经含有<code class="cd">atom.xml</code>这个文件，在格式<code class="cd">entry</code>里添加<code class="cd">summary</code>，也就是每篇博客的摘要部分。下面我介绍一下上面需要注意的几个注意事项：

1）首先得现在你的配置文件<code class="cd">_config.yml</code>中配置一下属性site.production_url、site.JB.atom_path、site.author.name、site.author.email，不然就无法使用这些参数属性，可能会影响后面搜索功能的使用。如果你不想设置，可以直接在上面选项中填写，比如&#123;{ site.title }}可以写一个你自己喜欢的名称。以下是我设置的内容，可以参考一下：
<div class="highlight">
  <pre>
    <code class="text">title : lym的博客
author :
  name : lym
  email : luyueming1989@gmail.com
  github : https://github.com/lym406365619

production_url : http://lym406365619.github.io

JB :
  BASE_PATH : false
  atom_path : /atom.xml</code></pre>
</div>

2） 在使用post.description选项时，在你需要发表的md文件里添加description选项，比如我这篇博客的设置：

配置好RSS feed后，你可以在本地运行Jekyll，访问[http://loaclhost:4000/atom.xml](http://loaclhost:4000/atom.xml)看看你本地的RSS文件是否有问题。如果出现你所有的文章以及摘要，那说明你打配置文件没有出错。接着将你的修改好的项目提交到Github上吧。

##2 进入Tapir提交RSS feed
Tapir的官网为[http://tapirgo.com](http://tapirgo.com/)，进入官网后，在Your RSS feed选项中填写atom.xml的访问网址，比如我的就是[http://lym406365619.github.io/atom.xml](http://lym406365619.github.io/atom.xml)。这里需要注意的是，你可以提交带有http的网址，也可以提交不含http的网址，因为生成的token并不是一样的。不过经本人测定，还是选择带有http的那个url，不然在你部署完成之后，搜索的时候可能会出现重复的两条记录，即同一篇博文出现两次。之后在下面选项填好你的E-mail地址，点击大按钮GO！就可以帮你自动生成一个token以及一个secret token。这边我们只需要记下第一个token就行，生成页面如下所示。

<img src="/img/blog/Tapir_token.png" width="561px" height="226px" alt="Tapir生成的token" class="pic"></img>

##3 加入search框以及JavaScript

在你需要搜索的界面加上一个search框，代码如下：
<div class="highlight">
  <pre>
    <code class="text"><span class="nt">&lt;form</span> class="navbar-search pull-right" action="search.html"<span class="nt">></span>
  <span class="nt">&lt;input</span> type="text" class="search-query" placeholder="Search"<span class="nt">></span>
<span class="nt">&lt;/form></span></code></pre>
</div>

由于搜索用到了JQuery，需要加入两个JS文件<code class="cd">jquery.min.js</code>和<code class="cd">jquery-tapir.min.js</code>。由于search.html是在加载时执行搜索，所以这两个JS文件必须在header中加载，我的代码如下：
<div class="highlight">
  <pre>
    <code class="text">&lt;script src="&#123;{ BASE_PATH }}/js/jquery-1.6.1.min.js"></script>
&lt;script src="&#123;{ BASE_PATH }}/js/jquery-tapir.min.js"></script></code></pre>
</div>

你可以在Github上下载这两个文件，使用<code class="cd">git clone</code>命令
<pre class="command-line">
  <span class="command">$ git clone git@github.com:TapirGo/jquery-plugin.git</span>
</pre>
##4 创建search.html

由于上面的action为<code class="cd">search.html</code>。在根目录下创建该文件，内容如下：
<div class="highlight">
  <pre>
    <code class="text">---
layout: default
title: 搜索结果
---
&#123;% include JB/setup %}
<span class="nt">&lt;div</span>class="post_cont"<span class="nt">></span> 
  <span class="nt">&lt;h1></span>搜索结果<span class="nt">&lt;/h1></span> 

  <span class="nt">&lt;div</span> id="search_results"<span class="nt">></span> 
  <span class="nt">&lt;/div></span>
  <span class="nt">&lt;script></span>
    $('#search_results').tapir({'token': '<span class="jm">519992c33f61b05b19000d6f</span>'});
  <span class="nt">&lt;/script></span>

<span class="nt">&lt;/div></span></code></pre>
</div>

上面的红色字体就是之前生成的token，用你生成的值代替它就行了。

按照上述方法部署之后，应该就能在站内实现全局静态搜索了。


