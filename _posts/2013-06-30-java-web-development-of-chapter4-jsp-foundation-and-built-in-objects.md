---
layout: post
title: 《Java Web开发实战1200例》实用案例之第四章JSP基础与内置对象
categories: [Java]
tags: [JSP, Java Web, JavaScript, 自定义标签]
description: JSP是Java Server Page的缩写，它是Servlet的扩展，是一种基于本文的程序。JSP的特点是HTML代码与Java程序共同存在，JSP提供了request、response、session、application、out、page、config、exception和pageContent9个内置对象，JSP设计自定义标签的目的就是为了实现HTML代码的重用
---
{% include JB/setup %}
#《Java Web开发实战1200例》实用案例之第四章JSP基础与内置对象

##1 JSP的基本应用
JSP是Java Server Page的缩写，它是Servlet的扩展，是一种基于本文的程序。JSP的特点是HTML代码与Java程序共同存在，在接收到用户请求时，服务器会处理Java代码片段，然后生成处理结果的HTML页再返回给客户端，客户端的浏览器将呈现最终页面效果。

###1.1 自定义错误页面
使用JSP中的page指令的errorPage属性自定义错误页面，代码如下：
{% highlight java %}
<%@ page errorPage="error.jsp" %>
{% endhighlight %}
其中error.jsp是一个专门负责处理异常的网页。

也可以应用web.xml文件中的error-page参数定义错误页面，应用该参数配置的错误页面将对所有页面有效。
{% highlight xml %}
<error-page>
	<error-code>404</error-code>
	<loaction>/error.jsp</loaction>
</error-page>
{% endhighlight %}

###1.2 在JSP脚本中插入JavaScript代码
在一些网站中添加注册信息是，注册成功后可能需要弹出一个“注册成功”的提示信息窗口，这就需要在JSP脚本中插入JavaScript的alert()方法来实现。

创建用户注册表单页index.jsp，将表单提交到save.jsp，关键代码如下：
{% highlight html %}
<form action="save.jsp" method="post">
	......
</form>
{% endhighlight %}

创建save.jsp页面，该页只是为了演示如何在JSP脚本中插入JavaScript代码，并没有真正对表单数据进行处理，代码如下：
{% highlight html %}
<%
	out.println("<script>alert('注册成功！');window.location.href='index.jsp';</script>")
%>
{% endhighlight %}

##2 JSP的内置对象

JSP提供了<strong>request</strong>、<strong>response</strong>、<strong>session</strong>、<strong>application</strong>、<strong>out</strong>、<strong>page</strong>、<strong>config</strong>、<strong>exception</strong>和<strong>pageContent</strong>9个内置对象。其中request对象、response对象、out对象、page对象、config对象、exception对象和pageContent对象的有效范围是当前页面，而session对象的有效范围是当前会话，即同一个客户端的所有页面；application对象的有效范围是当前应用，只要服务器不关闭，这个对象就有效。

###2.1 JSP编程--request
request内置对象表示的是调用JSP页面的请求。通常，request对象是javax.servlet.http.HttpServletRequest接口的一个实例

典型应用：

通过<strong>request.getParameter("paramName")</strong>可以获得form提交过来的参数值

通过<strong>request.setAttribute(String name, Object object)</strong>进行数据传递

###2.2 实现重定向页面

response.sendRedirect(String url)：重定向JSP文件

和<code class="cd">&lt;jsp:forward></code>的区别：

sendRedirect通过客户端发起二次申请，不同的request对象；jsp:forward是同一个request，在服务器内部转发。

###2.3 应用session对象实现用户登录
通过session对象可以存储或读取客户相关的信息，如用户名等。方法：
{% highlight java %}
session.setAttribute(String name, Object obj)	//用于将信息保存在session范围内

session.getAttribute(String name)		//获取保存在session范围内的信息

session.invalidate();		//销毁session
{% endhighlight %}

##3 JSP的自定义标签
JSP设计自定义标签的目的就是为了实现HTML代码的重用。
###3.1 带标签体的自定义标签

首先了解一下什么是带标签体的标签。

示例：<strong>&lt;test:mytag>xxxxx&lt;/test:mytag></strong>

说明：以上标签中间的xxxxx即为标签体

<code class="cd">javax.servlet.jsp.tagext.TagSupport</code>类，该类实现了<code class="cd">Tag</code>接口，用于创建不带标签体的自结束标签，这些标签中可以带属性。

<code class="cd">javax.servlet.jsp.tagext.BodyTagSupport</code>类，该类继承了<code class="cd">TagSupport</code>，用于创建带标签体的标签。

开发和使用一个JSP自定义标签的过程：

<strong>1）</strong> 开发标记处理类，编译生成class 文件，该类要继承TagSupport 或BodyTagSupport；

<strong>2）</strong> 创建标记库描述符文件*.tld，在该文件中为标记处理类指定标签名、声明标签属性；

<strong>3）</strong> 在JSP中引用标签库；

<strong>4）</strong> 在JSP中使用JSP标签。

本实例主要使用标签体的getBodyContent()方法自定义一个标签，把一段字母按大写输出。

<strong>1）创建BodyTagSupport类的实现类TagBody.java。</strong>代码如下：
{% highlight java %}
package info.luyueming.bodycontent;

import java.io.IOException;
import javax.servlet.jsp.JspException;
import javax.servlet.jsp.tagext.BodyTagSupport;

public class TagBody extends BodyTagSupport {

	public int doEndTag() throws JspException {
		String ct = this.getBodyContent().getString();
		// 获取标签体的代码
		try {
			// 以大写输出
			this.pageContext.getOut().print(ct.toUpperCase());
		} catch (IOException e) {
			e.printStackTrace();
		}
		return EVAL_PAGE;
	}
	
}
{% endhighlight %}
<strong>2）创建tagbody.tld文件，并放在WEB-INF目录下。</strong>代码如下：
{% highlight xml %}
<tlib-version>1.0</tlib-version>
<short-name>toUpperCase</short-name>
<tag>
	<name>toupp</name>
	<tagclass>info.luyueming.bodycontent.TagBody</tagclass>
	<bodycontent>JSP</bodycontent>
</tag>
{% endhighlight %}
<strong>3）创建index.jsp页面。</strong>主要代码如下：
{% highlight html %}
  <body>
  		<toUpperCase:toupp>hello welcome!!!!</toUpperCase:toupp>  	
  </body>
{% endhighlight %}  
并在该jsp页面前加上下面一行，否则会报错。
{% highlight java %}
<%@ taglib uri="/WEB-INF/tagbody.tld"  prefix="toUpperCase"%>
{% endhighlight %}  

##4 总结
这一章主要介绍了JSP的基本应用、JSP九个内置对象、以及JSP的自定义标签三个方面。其中九个内置对象是非常重要的，值得进一步去学习。