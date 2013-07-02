---
layout: post
title: 《Java Web开发实战1200例》实用案例之第五章JavaBean技术
categories: [Java]
tags: [JavaBean, Java Web]
description: 本章主要介绍了JavaBean技术的相关应用，有字符串处理、数据验证、日期时间处理、输出实用的HTML代码、窗口与对话框以及对数据库操作的JavaBean
---
{% include JB/setup %}
#《Java Web开发实战1200例》实用案例之第五章JavaBean技术

##1 字符串转换成数组
将字符串转换为数组也是开放中常用的技术，如用户在某个表单页中选中了复选框，在处理这些选中的内容时，需要获得复选框的所选中的内容，此时需要JavaScript方法将所有选中的内容累加，然后将这个累加后的字符串提交，在服务器端获得该字符串时，就需要将这个包含多个复选框内容的长字符串分解为数组，然后进一步处理。

可以通过<strong>String</strong>类的<code class="cd">split()</code>方法来实现将字符串转换为数组。split()方法包含一个String类型的参数regex，调用时会以regex为字符串的分隔符，将字符串分隔为字符串数组。

##2 整型与字符串相互转换
在实际开发过程中，经常需要将整型值转换为字符串。可以使用以下3中渐变的方法：
{% highlight java %}
String.valueOf()

Integer.toString()

Integer.valueOf().toString();
{% endhighlight %}
将字符串转型为整型，主要使用的是<strong>Integer</strong>类的<code class="cd">parseInt()</code>方法和<code class="cd">valueOf()</code>方法。它们都可以接收一个String类型的参数。

##3 数据验证（也可以使用正则表达式）

###3.1 判断字符串是否以指定字符开头
{% highlight java %}
public boolean startsWith(String prefix)
{% endhighlight %}

###3.2 检查字符串是否包含英文字母

应用<strong>String</strong>类的<code class="cd">toCharArray()</code>方法。首先通过该方法将指定的字符串转换为字符数组，然后循环该字符数组。其中大写英文字母的ASCII码范围在65~90之间，小写的在97~112之间。

###3.3 判断用户输入的日期是否为当前日期

主要应用<strong>java.util.Calendar</strong>类实现。
{% highlight java %}
Calendar now = Calendar.getInstance();	//创建一个当前时间的Calendar对象
int year =  now.get(now.YEAR);	//获得当前时间的年份
int month = now.get(now.MONTH) + 1;	//获得当前时间的月份。默认的月份是从0开始的，所以需要加1
int date = now.get(now.DAY_OF_MONTH);	//获得当前时间的日
int hour = now.get(now.HOUR_OF_DAY);	//获得当前时间的小时
int minute = now.get(now.MINUTE);	//获得当前时间的分钟
int second = now.get(now.SECOND);	//获得当前时间的秒
{% endhighlight %}

##4 日期时间处理
###4.1 将指定日期字符串转换为Calendar对象

主要应用格式化日期短时间的<strong>java.text.SimpleDateFormat</strong>类，实现步骤如下：
{% highlight java %}
//创建一个“yyyy-mm-dd”格式的格式化对象
SimpleDataFormat format = new SimpleDateFormat("yyyy-MM-dd");
//通过SimpleDateFormat对象的parse()方法将制定字符串转换为Date对象
Date date = format.parse("2013-07-01");
//通过Calendar对象的setTime()方法将Date对象转换为Calendar对象
Calendar calendar = Calendar.getInstance();
calendar.setTime(date);
{% endhighlight %}

###4.2 获得系统当前时间的字符串格式

通过<strong>java.util.Calendar</strong>类以及<strong>java.util.Date</strong>类来获得系统的当前时间

使用<code class="cd">Calendar</code>类，要通过该类对象的get()方法获得时间中的年、月、日、小时、分钟和秒，然后将这些值组成一个字符串。

使用<code class="cd">Date</code>类，可以通过<strong>java.text.SimpleDateFormat</strong>类将一个Date对象格式化为指定格式的日期时间字符串。代码如下：
{% highlight java %}
Date date = new Date();
SimpleDataFormat format = new SimpleDateFormat("yyyy-MM-dd hh-mm-ss");
String dateStr = format.format(date);
{% endhighlight %}

###4.3 计算出两个日期相差的天数

主要应用<strong>Calendar</strong>对象中的<code class="cd">getTimeMillis()</code>方法，此方法返回一个long型的时间值，以毫秒为单位。首先获得两个日期对象的long类型的时间值，然后这两个值相减之后再除以一天的毫秒值<code class="cd">(1000*60*60*24)</code>，得到的值取整即两个日期相差的天数。

##5 输出分页导航的方法
在实际开发过程中，很多的功能模块显示数据的部分都需要分页显示，而且分页部分的HTML代码都是相同的，为了提高开发效率以及便于维护，可以将这部分分页导航的代码封装在JavaBean中。

新建名为Page的JavaBean类，该类用于封装分页导航的代码。
{% highlight java %}
package info.luyueming.bean;

public class Page {
	private int pageSize = 10; // 每页显示的记录数
	private int currentPage = 1; // 当前页
	private int totalPage = 0; // 总页数
	private int totalRows = 0; // 总记录数
	private boolean hasBefore = false; // 是否有上一页
	private boolean hasNext = false; // 是否有下一页
	private String linkHTML = ""; // 用于保存分页导航的HTML代码
	private String pageURL; // 具体的链接地址

	public int getPageSize() {
		return pageSize;
	}

	public void setPageSize(int pageSize) {
		this.pageSize = pageSize;
	}

	public int getCurrentPage() {
		return currentPage;
	}

	public void setCurrentPage(int currentPage) {
		this.currentPage = currentPage;
	}

	public int getTotalPage() {
		totalPage = ((totalRows + pageSize) - 1) / pageSize;	// 根据数据总数和每页显示的记录数算出总页数
		return totalPage;
	}

	public int getTotalRows() {
		return totalRows;
	}

	public void setTotalRows(int totalRows) {
		this.totalRows = totalRows;
	}

	public boolean isHasBefore() {
		return hasBefore;
	}

	public void setHasBefore(boolean hasBefore) {
		this.hasBefore = hasBefore;
	}

	public boolean isHasNext() {
		return hasNext;
	}

	public void setHasNext(boolean hasNext) {
		this.hasNext = hasNext;
	}

	public String getPageURL() {
		return pageURL;
	}

	public void setPageURL(String pageURL) {
		this.pageURL = pageURL;
	}

	// 单击的是首页
	public void firstPage() {
		currentPage = 1; // 当前页的值为1
		this.setHasBefore(false); // 没有上一页
		this.refresh(); // 单击首页时应该设置是否有上一页和下一页
	}

	// 单击的是上一页
	public void beforePage() {
		currentPage--; // 当前页的值减1
		this.refresh(); // 单击上一页时应该设置是否有上一页和下一页

	}

	// 单击的是下一页
	public void nextPage() {
		if (currentPage < totalPage) {
			currentPage++; // 当前页的值加1
		}
		this.refresh(); // 单击下一页时应该设置是否有上一页和下一页
	}

	// 单击的是尾页
	public void lastPage() {
		currentPage = totalPage; // 当前页的值等于总页数
		this.setHasNext(false); // 没有下一页
		this.refresh(); // 单击上一页时应该设置是否有上一页和下一页
	}

	// 判断用户的操作，判断是否有上一页和下一页
	public void refresh() {
		if (totalPage <= 1) { // 总页数小于等于1的情况，没有上一页和下一页
			this.setHasBefore(false);
			this.setHasNext(false);
		} else if (currentPage == 1) { // 当前页为首页，没有上一页，有下一页
			this.setHasBefore(false);
			this.setHasNext(true);
		} else if (currentPage == totalPage) {// 当前页为尾页，没有下一页，有上一页
			this.setHasBefore(true);
			this.setHasNext(false);
		} else {// 除了以上的所有条件，有上一页和下一页
			this.setHasBefore(true);
			this.setHasNext(true);
		}
	}

	// 获得分页导航代码的方法，主要根据是否有上一页和下一页来判断
	public String getLinkHTML() {
		linkHTML += "共" + this.totalRows + "条记录 &nbsp;&nbsp;&nbsp;&nbsp;";
		if (this.hasBefore) {// 如果有上一页，添加上一页的超链接代码
			linkHTML += "<a href='" + this.pageURL + "?currPage=1'>首页</a>";
			linkHTML += "&nbsp;&nbsp;&nbsp;&nbsp;";
			linkHTML += "<a href='" + this.pageURL + "?currPage=" + this.currentPage + "&action=before'>上一页</a>";
			linkHTML += "&nbsp;&nbsp;&nbsp;&nbsp;";
		} else { // 如果没有上一页
			linkHTML += "首页  &nbsp;&nbsp;&nbsp;&nbsp;上一页&nbsp;&nbsp;&nbsp;&nbsp;";
		}
		if (this.hasNext) { // 如果有下一页，添加下一页的超链接代码
			linkHTML += "<a href='" + this.pageURL + "?currPage=" + this.currentPage + "&action=next'>下一页</a>";
			linkHTML += "&nbsp;&nbsp;&nbsp;&nbsp;";
			linkHTML += "<a href='" + this.pageURL + "?currPage=" + this.totalPage + "'>尾页</a>";
			linkHTML += "&nbsp;&nbsp;&nbsp;&nbsp;";
		} else { // 没有下一页
			linkHTML += "下一页  &nbsp;&nbsp;&nbsp;&nbsp;尾页&nbsp;&nbsp;&nbsp;&nbsp;";
		}
		linkHTML += "当前为" + this.currentPage + "/" + this.totalPage + "页";
		return linkHTML;
	}

	public void setLinkHTML(String linkHTML) {
		this.linkHTML = linkHTML;
	}
}
{% endhighlight %}

##5 窗口与对话框
在网站开发中，经常需要对网站的某个功能的操作给出提示信息的对话框或者弹出一个固定大小窗口，以便于用户正确的使用网站

###5.1 打开指定大小的新窗口
使用JavaScript打开一个弹出窗口，可以使用<strong>window</strong>对象的<code class="cd">open()</code>方法或者<code class="cd">showModalDialog()</code>方法完成。

<strong>showModalDialog()</strong>方法的代码如下：
{% highlight javascript %}
var url="window1.jsp"
window.showModalDialog(url,'window','status=no;dialogWidth=560px;dialogHeight=190px;center=yes;help=no;location=no;');
{% endhighlight %}
<strong>open()</strong>方法如下：
{% highlight javascript %}
var url="window1.jsp"
window.open(url,'window','status=no,width=560px,height=190px,center=yes,help=no,location=no,menubar=no,toolbar=no');
{% endhighlight %}
使用<strong>showModalDialog()</strong>方法弹出的窗口是模态窗口，不能最小化，只有关闭当前弹出的模态窗口才能操作其他页面。而使用<strong>open()</strong>方法弹出的更像是一个页面，此时对操作其他页面没有影响，但是在弹出该页面之前会首先弹出一个警告对话框，从而会给网站浏览者带来不必要的操作。

##6 总结
本章主要介绍了JavaBean技术的相关应用，有字符串处理、数据验证、日期时间处理、输出实用的HTML代码、窗口与对话框以及对数据库操作的JavaBean。其中日期时间处理是数据库中经常用到的，必须学会Date、Calendar以及字符串三者之间的相互转换；数据验证这一节可以使用正则表达式来代替实现；如何学会使用分页也是本章的一个重点。