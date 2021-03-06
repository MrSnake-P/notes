[TOC]

## 理解面向对象的编程

​		面向对象的世界引入了对象的概念 ，而这些又具有属性（数据成员) 和过程（成员函数）。这些函数的作用是处理属性。

​		**在 Python 中，一切皆为对象**。每个类的实例或变量都有它的内存地址或身份。对象就是类的实例，应用开发就是通过让对象交互来实现目的的过程。

## 面向对象编程的主要概念

1.封装

* 对象的行为对外部世界是不可见的。
* 客服端不能直接操作修改对象的内部，只能通过对象设置的成员函数。如，get、set。
* Python，在封装上不是隐式的，但可以同在变量或函数名前加前缀`__`,访问性变为私有。

2.多态

* 多态的两种类型
  * 对象根据输入参数提供方法的不同实现。
  * 不同类型的对象可以使用相同的接口。
* Python，多态是该语言的内置功能。如，操作符 “+” ，既可以加法运算，也可以连接字符串。

3.继承

* 继承表示可以一个类可以继承父类的大部分功能。
* 继承可以对原始软件的实现进行独立的扩展。
* 继承可以利用不同类的对象之间的关系建立层次结构，Python 支持多重继承。

4.抽象

* 提供简单的客服端接口，与类的对象进行交互，并可以调用定义的各个方法。
* 将内部类的复杂性抽象为一个接口，客户端不需要知道内部实现。

## 面向对象的设计原则