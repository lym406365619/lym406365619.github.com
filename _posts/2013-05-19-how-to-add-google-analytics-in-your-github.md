---
layout: post
title: 如何在Github托管网站中添加谷歌分析（Google Analytics）——异步跟踪
categories: [Github]
tags: [Google, Analytics, ssh, tracking_id, 异步跟踪, Firefox]
description: 首先了解一下什么是谷歌分析，进入中文官网请点击这里。当拥有自己个人网站的时候，我们就可以利用它来评估广告的投资回报率，并对您的Flash、视频以及社交网络网站和应用进行跟踪等功能。这里我主要介绍如何在Github上配置Google分析
---
{% include JB/setup %}
#如何在Github托管网站中添加谷歌分析（Google Analytics）——异步跟踪
首先了解一下什么是谷歌分析，进入中文官网请点击[这里](http://www.google.cn/intl/zh-CN/analytics/)。

当拥有自己个人网站的时候，我们就可以利用它来评估广告的投资回报率，并对您的Flash、视频以及社交网络网站和应用进行跟踪等功能。这里我主要介绍如何在Github上配置Google分析。

##1 创建Github二级域名
当然，如果想拥有一个个人域名的网站，又不想花钱去买域名以及服务器，你可以在Github网站上注册账号。登入之后，你就可以创建新项目。在Github中，一个项目对应唯一的Git版本库。Github为每一个用户分配了一个二级域名username.github.com。比如我的二级域名就是[lym406365619.github.com](http://lym406365619.github.io)，我们可以很容易的创建自己二级域名中的主页，只要在托管空间下创建一个名为username.github.com的版本库，像其master分支提交网站静态页面即可，其中网站的首页为index.html。其中向Github添加SSH公钥可以参考一下我的这篇文章([http://lym406365619.github.io/blog/2013/05/16/add-ssh-keys-into-your-github/](http://lym406365619.github.io/blog/2013/05/16/add-ssh-keys-into-your-github/))。

##2 获得google分析的tracking_id
进入google分析[官网](http://www.google.cn/intl/zh-CN/analytics/)。登陆进去，依次点击管理 --> 新账户，选择想要跟踪的内容，这边我们选着网站，跟踪方法选择Universal Analytics,接着填入网站名称、网址等基本信息，点击获取跟踪ID，此时网站会总动帮你生成一个跟踪ID以及你的跟踪代码，下图是我的账户生成的跟踪ID以及跟踪代码。

<img src="/img/blog/google_analytics.png" width="578px" height="431px" class="pic" alt="谷歌分析ID以及跟踪代码"></img>

##3 将Google分析部署到你的Github项目

###------设置tracking_id

打开<code>&#95;config.yml</code>配置文件，设置一下JB/analytics下的google的tracking-id，如下所示:
<div class="highlight">
  <pre>
    <code class="text">JB :
  analytics :
    provider : google 
    google : 
    tracking_id : '<em>your id</em>'</code></pre>
</div>
然后再向你需要运用Google分析的网站添加你刚才生成的javascript的跟踪代码。这样就能实现google分析跟踪设置。由于我想在我二级域名下的所有发表的博文都用google进行代码分析。因此参考[Jekyll Bootstrap](https://github.com/plusjade/jekyll-bootstrap)的设置，如果你本来就利用它的框架源码，就不要设置以下内容，只需改变一下上文中的跟踪ID即可。如果你设计了自己的主题框架，在需要进行google分析的页面中添加以下源码。比如我在<code>&#95;layout</code>下的default.html和post.html中添加如下代码：

<div class="highlight">
  <pre>
    <code class="text">&#123;% include JB/analytics %}</code></pre>
</div>

注意：&#123;% include JB/analytics %}应放在相应网页作为结束标记的<code>&#60;/head></code>标记之前，且紧邻该标记。如果您的网站是使用模板来生成网页，请将代码段添加到包含<code>&#60;head></code>部分的文件中，放在作为结束标记的 <code>&#60;/head></code>标记之前，且紧邻该标记。要在所有浏览器中都获得最佳效果，当在网站中加入其他脚本时，建议您采用以下两种方式之一：

1） 放到HTML<code>&#60;/head></code>部分的跟踪代码段之前；

2） 在跟踪代码段与所有网页内容之后（如置于HTML正文的底部）。

其中analytics位于_layouts下的JB文件夹里，代码为：
<div class="highlight">
  <pre>
     <code class="text">&#123;% <span class="ck">if</span> site.safe and site.JB.analytics.provider and page.JB.analytics != false %}

  &#123;% <span class="ck">case</span> site.JB.analytics.provider %}
  &#123;% <span class="ck">when</span> "google" %}
    &#123;% include JB/analytics-providers/google %}
  &#123;% <span class="ck">when</span> "getclicky" %}
    &#123;% include JB/analytics-providers/getclicky %}
  &#123;% <span class="ck">when</span> "mixpanel" %}
    &#123;% include JB/analytics-providers/mixpanel %}
  &#123;% <span class="ck">when</span> "custom" %}
    &#123;% include custom/analytics %}
  &#123;% <span class="ck">endcase</span> %}

&#123;% <span class="ck">endif</span> %}</code></pre>
</div>
因为之前<code>&#95;config.yml</code>配置文件中设置为google，所以应该执行JB/analytics-providers/google里面的代码。下面应该是添加JavaScript代码里。

###------设置JavaScript代码
刚开始我把之前生成的JavaScript代码复制到<code>&#95;include/JB/analytics-providers/google</code>里面，但这个时候问题出现了，我利用谷歌调试工具进行测试，发现代码没有生效（一会我会告诉大家如何知道自己的跟踪已经设置成功了），不知道什么原因，而且我也是按照官方的设置进行部署的。于是我就使用JB使用的异步跟踪设置方式，发现测试成功。代码如下：
<div class="highlight">
  <pre>
    <code class="text">&#60;script type="text/javascript">

  <span class="jk">var</span> _gaq = _gaq || [];
  _gaq.<span class="jm">push</span>([<span class="jv">'_setAccount'</span>, <span class="jv">'{{ site.JB.analytics.google.tracking_id }}'</span>]);
  _gaq.<span class="jm">push</span>(['_trackPageview']);

  (<span class="jk">function</span>() {
    <span class="jk">var</span> ga = document.<span class="jm">createElement</span>('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    <span class="jk">var</span> s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();

&#60;/script></code></pre>
</div>


##4 调试、检测谷歌异步跟踪
Google分析官方提供了[跟踪代码问题指导](https://developers.google.com/analytics/resources/articles/gaTrackingTroubleshooting?hl=zh-CN)，列出常见的跟踪代码错误、常见报告数据问题以及调试方法。

下表是不同浏览器所使用不同的测测试调试工具：

浏览器 | 调试工具
:------|:----------
All | [Firebug Lite](http://getfirebug.com/firebuglite)
Chrome | [Google Analytics Tracking Code Debugger](https://chrome.google.com/extensions/detail/jnkmfdileelhofjcijamephohjechhna)
Chrome | Built-in Developer Tools (e.g. [Resources](http://www.chromium.org/devtools/google-chrome-developer-tools-tutorial#resources))
Firefox | [Live HTTP Headers](http://livehttpheaders.mozdev.org/installation.html)
Firefox | [Firebug](https://addons.mozilla.org/en-US/firefox/addon/1843)
IE | [Fiddler](http://www.fiddlertool.com/fiddler/)
Sarari | Built-in Activity window
IE/Sarari/Firefox | [Charles](http://www.xk72.com/charles/download.php)

我使用的是Firefox,介绍一下如何在Firefox调试谷歌异步跟踪是否正常运行，我使用的是[Live HTTP Headers](http://livehttpheaders.mozdev.org/installation.html)工具

<strong>第一步：</strong>下载并安装[Live HTTP Headers](http://livehttpheaders.mozdev.org/installation.html)；安装好后重启浏览器，一般会提示你重启浏览器；

<strong>第二步：</strong>打开Live HTTP Headers插件，在菜单栏选择Tools(工具) --> LiveHTTPHeaders；

<strong>第三步：</strong>点击打开页面上的Generator选项，并选择右边选项中的images、CSS、request；

<strong>第四步：</strong>打开含有Google分析跟踪插件的页面，即你要检测的页面。如果你之前已经打开了，刷新页面就行；

<strong>第五步：</strong>在刷新进入页面后，验证页面是否对<code>urchin.js</code>或者<code>ga.js</code>发出请求；

<strong>第六步：</strong>检查是否对<code>_utm.gif</code>发出请求。

如果实现上述步骤里，说明你的追踪设置应该没有问题。

如果想使用其他浏览器进行检测，请看[官方文档](https://developers.google.com/analytics/resources/articles/gaTrackingTroubleshooting?hl=zh-CN)吧。

以下是我测试页面的结果:

<img src="/img/blog/google_tracking.png" width="500px" height="387" alt="Google跟踪分析调试结果" class="pic"></img>

##5 结语
个人提交Google跟踪分析已经两天了，不过我的Google跟踪状态还是“未安装跟踪代码”，上网搜了以下，好像确实要等很久，如果各位也像我一样，不用着急，慢慢等吧。不过网站的一些数据还是可以看到的，因为Google会自动帮你生成一个报告分析，看了一下还是很详细的，可以慢慢研究。