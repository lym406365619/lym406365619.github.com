---
layout: post
title: Java中抽象类和接口的区别
categories: [Java]
tags: [Interface, Abstract Class]
description: 在Java语言中，抽象类（abstract class）和接口（interface）是支持抽象类定义的两种机制。搞清楚两者的区别还是挺重要的，对于它们的选择甚至反映出对于问题领域本质的理解，对于设计意图的理解是否正确、合理
---
{% include JB/setup %}
# Java中抽象类和接口的区别
在Java语言中，抽象类（<strong>abstract class</strong>）和接口（<strong>interface</strong>）是支持抽象类定义的两种机制。虽然两者在对于抽象类定义的支持方面有很大的相似性，甚至可以互相替换。其实，搞清楚两者的区别还是挺重要的，对于它们的选择甚至反映出对于问题领域本质的理解，对于设计意图的理解是否正确、合理。本文将对两者之间的区别进行分析。

##1 抽象类简介
<strong>abstract class</strong>和<strong>interface</strong>在Java语言中都是用来描述抽象类（本文中的抽象类并非从abstract class翻译而来，它表示的是一个抽象体，而abstract class为Java语言中用于定义抽象类的一种方法，请读者注意区分）定义的，那么什么是抽象类，使用抽象类能为我们带来什么好处呢？

在面向对象的概念中，我们知道所有的对象都是通过类来描绘的，但是反过来却不是这样。并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是<strong>抽象类</strong>。<strong>抽象类往往用来表征我们在对问题领域进行分析、设计中得出的抽象概念，是对一系列看上去不同，但是本质上相同的具体概念的抽象。</strong>比如：如果我们进行一个图形编辑软件的开发，就会发现问题领域存在着圆、 三角形这样一些具体概念，它们是不同的，但是它们又都属于形状这样一个概念，形状这个概念在问题领域是不存在的，它就是一个抽象概念。正是因为抽象的概念 在问题领域没有对应的具体概念，所以用以表征抽象概念的抽象类是不能够实例化的。

在面向对象领域，抽象类主要用来进行类型隐藏。我们可以构造出一个固定的一组行为的抽象描述，但是这组行为却能够有任意个可能的具体实现方式。这个抽象描述就是抽象类，而这一组任意个可能的具体实现则表现为所有可能的派生类。模块可以操作一个抽象体。由于模块依赖于一个固定的抽象体，因此它可以是不允许修改的；同时，通过从这个抽象体派生，也可扩展此模块的行为功能。熟悉OCP的读者一定知 道，为了能够实现面向对象设计的一个最核心的原则OCP(Open-Closed Principle)，抽象类是其中的关键所在。

##2 从语法定义层面看abstract class和interface

在语法层面，Java语言对于<strong>abstract class</strong>和<strong>interface</strong>给出了不同的定义方式，下面以定义一个名为Demo的抽象类为例来说明这种不同。

使用abstract class的方式定义Demo抽象类的方式如下：
{% highlight java %}
abstract class Demo {
	abstract void method1();
	abstract void method2();
}
{% endhighlight %}

使用interface的方式定义Demo抽象类的方式如下：
{% highlight java %}
interface Demo {
	void method1();
	void method2();
}
{% endhighlight %}

在abstract class方式中，可以有自己的数据成员，也可以有非abstract的成员方法，而在interface方式的实现中，只能够有静态的不能被修改的数据成员（也就是必须是static final的，不过在interface中一般不定义数据成员），所有的成员方法都是abstract的。<strong>从某种意义上说，interface是一种特殊形式的abstract class</strong>。

##3 从编程的角度来看abstract class和interface

从编程的角度来看，<strong>abstract class</strong>和<strong>interface</strong>都可以用来实现“design by contract”的思想。但是在具体的使用上面还是有一些区别的。

首先，abstract class在 Java 语言中表示的是一种继承关系，一个类只能使用一次继承关系(因为Java不支持多继承)。但是，一个类却可以实现多个interface。

其次，在abstract class的定义中，我们可以赋予方法的默认行为。但是在interface的定义中，方法却不能拥有默认行为，为了绕过这个限制，必须使用委托，但是这会增加一些复杂性，有时会造成很大的麻烦。

##4 从设计理念层面看abstract class和interface

<strong>abstract class</strong>在Java语言中体现了一种继承关系，要想使得继承关系合理，父类和派生类之间必须存在“is-a”关系，即父类和派生类在概念本质上应该是相同的。对于<strong>interface</strong>来说则不然，并不要求interface的实现者和interface定义在概念本质上是一致的，仅仅是实现了interface定义的契约而已。为了使论述便于理解，下面将通过一个简单的实例进行说明。

考虑这样一个例子，假设在我们的问题领域中有一个关于Door的抽象概念，该Door具有执行两个动作open和close，此时我们可以通过abstract class或者interface来定义一个表示该抽象概念的类型，定义方式分别如下所示：

使用<strong>abstract class</strong>方式定义Door：
{% highlight java %}
abstract class Door {
	abstract void open();
	abstract void close();
}
{% endhighlight %}

使用<strong>interface</strong>方式定义Door：
{% highlight java %}
interface Door {
	void open();
	void close();
}
{% endhighlight %}

其他具体的Door类型可以<code class="cd">extends</code>使用<strong>abstract class</strong>方式定义的Door或者<code class="cd">implements</code>使用<strong>interface</strong>方式定义的Door。看起来好像使用abstract class和interface没有大的区别。

如果现在要求Door还要具有报警的功能。我们该如何设计针对该例子的类结构呢（在本例中，主要是为了展示abstract class和interface反映在设计理念上的区别，其他方面无关的问题都做了简化或者忽略）？下面将罗列出可能的解决方案，并从设计理念层面对这些不同的方案进行分析。

### 解决方案一
简单的在Door的定义中增加一个alarm方法，如下：
{% highlight java %}
abstract class Door {
	abstract void open();
	abstract void close();
	abstract void alarm();
}
{% endhighlight %}

或者
{% highlight java %}
interface Door {
	void open();
	void close();
	void alarm();
}
{% endhighlight %}


那么具有报警功能的AlarmDoor的定义方式如下：
{% highlight java %}
class AlarmDoor extends Door {
	void open() {/*...*/}
	void close() {/*...*/}
	void alarm() {/*...*/}
}
{% endhighlight %}
或者
{% highlight java %}
class AlarmDoor implements Door {
	void open() {/*...*/}
	void close() {/*...*/}
	void alarm() {/*...*/}
}
{% endhighlight %}

这种方法违反了面向对象设计中的一个核心原则ISP(Interface Segregation Principle)，在Door的定义中把Door概念本身固有的行为方法和另外一个概念“报警器”的行为方法混在了一起。这样引起的一个问题是那些仅仅依赖于Door这个概念的模块会因为“报警器”这个概念的改变（比如：修改alarm方法的参数）而改变，反之依然。

###解决方案二
既然open、close和alarm属于两个不同的概念，根据ISP原则应该把它们分别定义在代表这两个概念的抽象类中。定义方式有：这两个概念都使用abstract class方式定义；两个概念都使用interface方式定义；一个概念使用abstract class方式定义，另一个概念使用interface方式定义。

显然，由于Java语言不支持多重继承，所以两个概念都使用abstract class方式定义是不可行的。后面两种方式都是可行的，但是对于它们的选择却反映出对于问题领域中的概念本质的理解、对于设计意图的反映是否正确、合理。我们一一来分析、说明。

如果两个概念都使用interface方式来定义，那么就反映出两个问题：

<strong>1、</strong>我们可能没有理解清楚问题领域，AlarmDoor在概念本质上到底是Door还是报警器？

<strong>2、</strong>如果我们对于问题领域的理解没有问题，比如：我们通过对于问题领域的分析发现AlarmDoor在概念本质上和Door是一致的，那么我们在实现时就没有能够正确的揭示我们的设计意图，因为在这两个概念的定义上（均使用interface方式定义）反映不出上述含义。

如果我们对于问题领域的理解是：AlarmDoor在概念本质上是Door，同时它有具有报警的功能。我们该如何来设计、实现来明确的反映出我们的意思呢？前面已经说过，abstract class在Java语言中表示一种继承关系，而继承关系在本质上是“is-a”关系。所以对于Door这个概念，我们应该使用abstarct class方式来定义。另外，AlarmDoor又具有报警功能，说明它又能够完成报警概念中定义的行为，所以报警概念可以通过interface方式定义。如下所示：
{% highlight java %}
abstract class Door {
	abstract void open();
	abstract void close();
}

interface Alarm {
	void alarm();
}

class Alarm Door extends Door implements Alarm {
	void open() {/*...*/}
	void close() {/*...*/}
	void alarm() {/*...*/}
}
{% endhighlight %}
这种实现方式基本上能够明确的反映出我们对于问题领域的理解，正确的揭示我们的设计意图。其实<strong>abstract class</strong>表示的是“<strong>is-a</strong>”关系，<strong>interface</strong>表示的是“<strong>like-a</strong>”关系，大家在选择时可以作为一个依据，当然这是建立在对问题领域的理解上的，比如：如果我们认为AlarmDoor在概念本质上是报警器，同时又具有Door的功能，那么上述的定义方式就要反过来了。

##5 小结
<strong>1. </strong><strong>abstract class</strong>在Java语言中表示的是一种继承关系，一个类只能使用一次继承关系。但是，一个类却可以实现多个<strong>interface</strong>。

<strong>2. </strong>在<strong>abstract class</strong>中可以有自己的数据成员，也可以有非abstarct的成员方法，而在<strong>interface</strong>中，只能够有静态的不能被修改的数据成员（也就是必须是static final的，不过在 interface中一般不定义数据成员），所有的成员方法都是abstract的。

<strong>3. </strong><strong>abstract class</strong>和<strong>interface</strong>所反映出的设计理念不同。其实<strong>abstract class</strong>表示的是“<strong>is-a</strong>”关系，<strong>interface</strong>表示的是“<strong>like-a</strong>”关系。

<strong>4. </strong>实现抽象类和接口的类必须实现其中的所有方法。抽象类中可以有非抽象方法。接口中则不能有实现方法。

<strong>5. </strong>接口中定义的变量默认是<strong>public static final</strong>型，且必须给其初值，所以实现类中不能重新定义，也不能改变其值。

<strong>6. </strong>抽象类中的变量默认是<strong>friendly</strong>型，其值可以在子类中重新定义，也可以重新赋值。

<strong>7. </strong>接口中的方法默认都是<strong>public</strong>、<strong>abstract</strong>类型的。

##参考
[http://dev.yesky.com/436/7581936.shtml](http://dev.yesky.com/436/7581936.shtml)