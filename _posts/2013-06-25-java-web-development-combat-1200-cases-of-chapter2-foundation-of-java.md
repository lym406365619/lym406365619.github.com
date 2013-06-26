---
layout: post
title: 《Java Web开发实战1200例》实用案例之第2章Java语言基础（1）
categories: [Java]
tags: [Java Web. 位运算, 浮点数, BigDecimal, 杨辉三角]
description: 主要介绍使用位运算进行加密、精确使用浮点数——BigDecimal类、不使用第三个变量实现两个整型变量的互换、以及使用for循环输出杨辉三角四种案例，其中精确使用浮点数在银行使用的系统中是非常重要的，必须学会使用BigDecimal类中的四种基本方法
---
{% include JB/setup %}
#《Java Web开发实战1200例》实用案例之第2章Java语言基础（1）

##1 使用位运算进行加密

通过位运算的异或运算符“^”把字符串与一个指定的值进行异或运算，从而改变字符串每个字符的值，这样就可以得到一个加密后的字符串。当把加密后的字符串作为程序输入内容，异或运算会把加密后的字符串还原为原有字符串的值。程序如下：

{% highlight java %}
package info.luyueming.java;

import java.util.Scanner;

public class Example {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入一个英文字符串或者解密字符串");
		String password = scan.nextLine();		//获取输入
		char[] array = password.toCharArray();	//获取字符数组
		for(int i=0; i<array.length; i++) {		//遍历字符数组
			array[i] = (char) (array[i]^20000);	//对每个数组元素进行异或运算
		}
		System.out.println("加密或解密结果如下：");
		System.err.println(new String(array));	//输出密钥
	}
}
{% endhighlight %}

运行结果如下：

{% highlight yaml %}
请输入一个英文字符串或者解密字符串
http://luyueming.info
加密或解密结果如下：
么乔乔乐业丏丏乌乕乙乕久乍义乎乇与义乎乆乏

请输入一个英文字符串或者解密字符串
么乔乔乐业丏丏乌乕乙乕久乍义乎乇与义乎乆乏
加密或解密结果如下：
http://luyueming.info

{% endhighlight %}

##2 精确使用浮点数——BigDecimal类
在货币运算中，经常会涉及小数运算。在计算机中所有数字都是使用二进制进行存储的，而二进制无法精确地表示所有的小数，所以使用基本数据类型进行小数运算会有一些误差，本实例将通过<code class="cd">BigDecimal</code>类实现精确的小数运算。

BigDecimal类是在<code class="cd">java.math.*</code>类包中，在使用时需要引入该类包，该类主要有使用加、减、乘、除四种运算方法

{% highlight java %}
public BigDecimal add(BigDecimal value);                        //加法
public BigDecimal subtract(BigDecimal value);                   //减法 
public BigDecimal multiply(BigDecimal value);                   //乘法
public BigDecimal divide(BigDecimal value);                     //除法
{% endhighlight %}

下面看一下实例：
{% highlight java %}
package info.luyueming.java;

import java.math.BigDecimal;

public class AccurateFloat {
	public static void main(String[] args) {
		//一般算法
		double money = 2;
		double price = 1.1;
		double result = money - price;
		System.out.println("非精确计算");
		System.out.println("剩余金额：" + result);	//输出运算结果
		//精确浮点数的解决办法
		BigDecimal money1 = new BigDecimal("2");
		BigDecimal price1 = new BigDecimal("1.1");	
		BigDecimal result1 = money1.subtract(price1);	
		System.out.println("非精确计算");
		System.out.println("剩余金额：" + result1);	//输出精确结果
	}
}
{% endhighlight %}

输出结果：

{% highlight yaml %}
非精确计算
剩余金额：0.8999999999999999
非精确计算
剩余金额：0.9
{% endhighlight %}

##3 不使用第三个变量实现两个变量的互换
变量互换常见于数组排序算法中，当判断两个数组元素需要交互时，将创建一个临时变量来共同完成互换，临时变量的创建增加了系统资源的消耗。如果需要交换的是两个整数类型的变量，那么可以使用更高效的方法。如下所示：
{% highlight java %}
package info.luyueming.java;

import java.util.Scanner;

public class VariableExchange {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入变量A的值");
		long A = scan.nextLong();
		System.out.println("请输入变量B的值");
		long B = scan.nextLong();
		System.out.println("A=" + A + "\tB=" + B);
		System.out.println("执行变量互换...");
		A = A ^ B;
		B = B ^ A;
		A = A ^ B;
		System.out.println("A=" + A + "\tB=" + B);
	}
}
{% endhighlight %}

输出结果：

{% highlight yaml %}
请输入变量A的值
20
请输入变量B的值
30
A=20    B=30
执行变量互换...
A=30    B=20
{% endhighlight %}

异或^和其他位运算符并不会改变变量本身的值，即“A\^B;”没有任何意义。这里变量的互换主要应用了异或的公式：
{% highlight java %}
a ^ b ^ a = b
{% endhighlight %}

##4 使用for循环输出杨辉三角
杨辉三角由数字排列，可以把它看作一个数字表，其基本特性是两侧数值均为1，其他位置的数值是其正上方的数值与左上角数值之和。由于Java语言中的二维数组其实是一维数组的每个元素都是另一个一维数组，所以第二维数组的长度可以任意。这比其他语言的数组更灵活，而且多维数组也是如此。在本例中，创建一个二维数组，并指定二维数组的第一维长度，这个数组用于存放杨辉三角的数值表，再通过双层for循环来实现第二维数组的长度，然后计算整个数组的每个元素值。

{% highlight java %}
package info.luyueming.java;

public class YanghuiTriangle {

	public static void main(String[] args) {
		int triangle[][] = new int[8][];
		//遍历二维数组的第一层
		for(int i=0; i<triangle.length; i++) {
			triangle[i] = new int[i+1];
			//遍历第二层数组
			for(int j=0; j<triangle[i].length; j++) {
				//将两侧的数组元素赋值为1
				if(i == 0 || j == 0 || j == triangle[i].length - 1 ) {
					triangle[i][j] = 1;
				} else {
				//其他数值通过公式计算
					triangle[i][j] = triangle[i-1][j] + triangle[i-1][j-1];
				}
				//输出数组元素
				System.out.print(triangle[i][j] + "\t");
			}
			System.out.println();
		}
	}

}
{% endhighlight %}
输出结果如下：
{% highlight yaml %}
1
1    1
1    2    1
1    3    3    1
1    4    6    4    1
1    5    10   10   5    1
1    6    15   20   15   6    1
1    7    21   35   35   21   7    1
{% endhighlight %}