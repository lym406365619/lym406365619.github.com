---
layout: post
title: Hibernate4中buildSessionFactory()方法过时解决方案
categories: [Java]
tags: [hibernate, session, deprecated, buildsessionfactory, 设计模式]
description: 在使用Hibernate4时，我们会发现hibernate中的buildSessionFactory()方法已经过时了，官网建议使用 buildSessionFactory(ServiceRegistry)这个方法代替使用。下面代码为解决过时的代码，并采用了单例设计模式，并且是懒汉式单例类。与饿汉式单例类不同的是，懒汉式单例类在第一次被引用时将自己实例化
---
{% include JB/setup %}
#Hibernate4中buildSessionFactory()方法过时解决方案
在使用Hibernate4时，我们会发现hibernate中的<code class="cd">buildSessionFactory()</code>方法已经过时了，官网建议使用<code class="cd">buildSessionFactory(ServiceRegistry)</code>这个方法代替使用。之前的使用如下所示：

{% highlight java %}
	//此方法已经过时
	Configuration cfg = new Configuration();
	SessionFactory sf = cfg.configure().buildSessionFactory();
	Session session = sf.openSession();
{% endhighlight %}

下面代码为解决过时的代码，采用了单例设计模式，并且是懒汉式单例类。与饿汉式单例类不同的是，懒汉式单例类在第一次被引用时将自己实例化。如果加载器是静态的，那么在懒汉式单例类被加载时不会将自己实例化。
{% highlight java %}
//新方法
package com.tool.hibernate;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
import org.hibernate.service.ServiceRegistry;
import org.hibernate.service.ServiceRegistryBuilder;

public class HibernateUtils {

	private static SessionFactory factory;

	static {
		Configuration cfg = new Configuration().configure();
		ServiceRegistry sr = new ServiceRegistryBuilder().applySettings(cfg.getProperties()).buildServiceRegistry();
		factory = cfg.buildSessionFactory(sr);
	}

	public static SessionFactory getSessionFactory() {
		return factory;
	}

	public static Session getSession() {
		return factory.openSession();
	}

	public static void closeSession(Session session) {
		if (session != null) {
			if (session.isOpen()) {
				session.close();
			}
		}
	}

}
{% endhighlight %}