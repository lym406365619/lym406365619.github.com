---
layout: post
title: 《Java Web开发实战1200例》实用案例之第三章HTML/CSS技术
categories: [Java]
tags: [HTML, CSS, JavaScript, Cookie, marquee]
description: 这一章主要要介绍了HTML/CSS技术，包括页面效果、表格样式、鼠标样式、文字及列表样式、文字特效和图片滤镜特效。其中页面效果中的三个案例比较实用，包括网页换肤、滚动文字、CSS控制绝对定位三个案例
---
{% include JB/setup %}
#《Java Web开发实战1200例》实用案例之第三章HTML/CSS技术

##1 前言
这一章主要要介绍了HTML/CSS技术，包括页面效果、表格样式、鼠标样式、文字及列表样式、文字特效和图片滤镜特效。其中页面效果中的三个案例觉得还是有点实用的，至于后面几个方面的内容，稍微归纳一下。

表格样式主要讲述了CSS边框属性<code class="cd">border</code>；通过两个鼠标事件（<code class="cd">onMouseOver</code>和<code class="cd">onMouseOut</code>）改变style属性来实现改变边框颜色；实用JavaScript中提供的<code class="cd">setTimeOut()</code>方法实现表格外边框的霓虹灯效果；通过CSS样式的progid滤镜实现背景颜色渐变；使用JavaScript的<code class="cd">split()</code>方法实现表格的隔行变色（<code class="cd">stringObj.split(str)[rowIndex]</code>）和隔列变色（<code class="cd">stringObj.split(str)[cellIndex]</code>）；设置表格<code class="cd"><td></code>中的<code class="cd">title</code>属性来实现鼠标经过表格的提示信息。

鼠标样式主要介绍了cursor属性来实现不同的鼠标形状以及在CSS样式中设置cursor的url属性实现动画光标（动态光标是一个.ani文件）。

文字及列表样式介绍了CSS中的<code class="cd">text-decoration</code>属性；在页面中使用<code class="cd">&lt;rt></code>和<code class="cd">&lt;ruby></code>标签实现在文字上方标注说明标记；使用段落标签<code class="cd">&lt;p></code>中的<code class="cd">first-line</code>和<code class="cd">first-letter</code>属性；使用CSS中<code class="cd">&lt;ul></code>的<code class="cd">list-style-image</code>属性实现指定图标列表。

文字特效和图片滤镜特效主要介绍了CSS样式中的glow滤镜、dropshadow滤镜、shadow滤镜、light滤镜等的用法。

##1 网页换肤

在实现网页换肤时，如果需要在换肤后保持网页的风格，即使关闭浏览器也不会消失，这就需要将每次执行所调用的CSS样式文件的路径信息保存到客户端的Cookie中。实现换肤的几个步骤

1）编写不同风格的CSS样式文件；

2）在需要的页面中引用其中一个CSS样式文件作为网页默认的样式；
{% highlight html %}
<link id="myCss" href="orange.css" rel="stylesheet">
{% endhighlight %}

3）在&lt;script>标签中编写保存cookie信息的方法，用于将CSS样式文件的路径信息保存到客户端Cookie文件中；
{% highlight javascript %}
function writeCookie(csspath) {
	var today = new Date();
	var expires = new Date();
	expires.setTime(today.getTime() + 1000*60*60*24*30);	//有效期为30天
	var str = "cssPath=" + csspath + ";expires=" + expires.toGMTString() + ";"；
	document.cookie=str;
}
{% endhighlight %}

4）编写读取cookie信息的方法；
{% highlight javascript %}
function readCookie(cookieName) {
	var search = cookieName + "=";
	if(document.cookie.length > 0) {
		offset = document.cookie.indexOf(search);
		if(offset != -1) {
			offset += search.length;
			end = document.cookie.indexOf(";", offset);
			if(end != -1) {
				end = document.cookie.length;
				return unescape(document.cookie.substring(offset, end));
			}
		}
	}
}
{% endhighlight %}

5）编写超链接的onClick事件调用的方法；
{% highlight javascript %}
function change(type) {
	if(type == "orange") {
		document.getElementById("myCss").href="orange.css";
		writeCookie("orange.css")
	}
	if(type == "gray") {
		document.getElementById("myCss").href="gray.css";
		writeCookie("gray.css")
	}
}
{% endhighlight %}

6）在页面中超链接代码。
{% highlight html %}
<a href="#" onClick="change('orange')">橘色经典</a>
<a href="#" onClick="change('gray')">灰色畅想</a>
{% endhighlight %}

##2 滚动文字
实现滚动文字的特效，主要在页面中应用<code class="cd">&lt;marquee></code>标签。通过该标签的属性可以实现不同的滚动效果，其中最常用的属性是<code class="cd">direction</code>(滚动的方向)、<code class="cd">behavior</code>(滚动的方式)、<code class="cd">scrollamount</code>(滚动的速度)。

比如下面的代码：
{% highlight html %}
<marquee direction="left">
	滚动中......
</marquee>
{% endhighlight %}

效果如下：
<marquee direction="left">
	滚动中......
</marquee>

##3 CSS控制绝对定位
在CSS中提供了灵活的定位方法，常用的定位属性包括position、left、top、width、height等，其中<code class="cd">position</code>属性就是实现绝对定位的关键属性，它采用了3中定位方式，包括绝对定位（absolute）、相对定位（relative）和静态定位（static）。CSS代码如下：
{% highlight css %}
#top_Div {
	position: absolute;		/*绝对定位*/
	left: 20px;				/*距离左侧页面20px*/
	top: 10px;				/*距离顶部页面10px*/
}
#bottom_Div {
	position: absolute;		/*绝对定位*/
	left: 130px;			/*距离左侧页面130px*/
	top: 10px;				/*距离顶部页面10px*/
}
{% endhighlight %}