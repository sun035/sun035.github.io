---
layout: post
title: java final
description: java final内容
category: blog
---

final关键字的含义：
---

&emsp;final为java保留关键字.可作用于类，方法 ，变量中，当用final来定义引用时,一旦初始化后便不可再被更改，
编译器会检查代码，如果你试图将变量再次初始化的话，编译器会报编译错误。
&emsp;被final修饰的效率要快，因为它是静态绑定的不需要在运行时在进行绑定

final变量：
---

&emsp;凡是用final进行修饰的成员变量或本地变量，其引用便不可以被改变，final经常和static一同修饰
作为常量


final方法：
---
&emsp;被final修饰的方法不可再被重写,当一个方法是完整的不需要在进行重写的时候使用final对方法进行定义


final类：
---

final修饰的类不可以被继承，例如java中的string



final关键字的好处
---

下面总结了一些使用final关键字的好处

final关键字提高了性能。JVM和Java应用都会缓存final变量。

final变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销。

使用final关键字，JVM会对方法、变量及类进行优化。

不可变类
---

创建不可变类要使用final关键字。不可变类是指它的对象一旦被创建了就不能被更改了。String是不可变类的代表。
不可变类有很多好处，譬如它们的对象是只读的，可以在多线程环境下安全的共享，不用额外的同步开销等等。










[Mukosame]:    http://sun035.github.io  "Mukosame"
