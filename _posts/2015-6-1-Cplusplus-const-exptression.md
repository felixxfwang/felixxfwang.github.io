---
layout: post
category: C++学习笔记
title: C++中的常量表达式 constexpr
tags: 
- C++
---


C++11标准引入的常量表达式（Constant Expressions）是指值不能改变且值能在编译期计算出来的表达式。

{% highlight c++ %}
constexpr int mf = 20;
constexpr int limit = mf + 1;
constexpr int sz = size();     // size()必须是被constexpr修饰
{% endhighlight %}

constexpr修饰的变量默认是const的。

## constexpr函数（constexpr function）

1、constexpr函数是可以用在常量表达式中的函数。

2、constexpr函数必须满足以下条件：

* 返回类型和所有参数的类型必须都是字面值类型（literal type）；
* 函数体只能只能有一个return语句；
* 不允许有产生运行时动作的语句。

3、constexpr函数有以下特点：

* 用constexpr修饰
* 编译期能确定返回值
* 对constexpr函数的调用在编译期替换为函数的返回结果
* constexpr函数默认是inline函数
* constexpr函数并不要求一定返回一个常量表达式。但是在需要一个常量表达式的环境中使用constexpr函数，那么它必须返回一个常量表达式，否则会报错。

{% highlight c++ %}
constexpr int new_sz() { return 42; }
constexpr int foo = new_sz();
constexpr size_t scale(size_t cnt) { return new_sz() * cnt; }     // 如果cnt是常量，则scale是constexpr函数

int arr0[scale(2)];     // 正确
int i = 2;
int arr[scale(i)];     // 错误，scale(i)不是常量表达式
{% endhighlight %}


## 字面值类型（literal type） 

常量表达式的值需要在编译时就得到计算，因此对声明constexpr时用到的类型必须是字面值类型。

#### 字面值类型主要包括两类：  
1、算术类型、引用和指针都属于字面值类型。  

2、类类型也可以是字面值类型，这取决于类的定义：  
(1)对聚合类型来说，如果它的数据成员全都是字面值类型，那么它就是字面值类型；  
(2)对非聚合类型来说，它必须满足以下条件：  

  * 所有的数据成员都是字面值类型；
  * 至少包含一个constexpr构造函数；
  * 如果含有in-class initializer，内置类型的初始化值必须是一个常量表达式，类类型的初始化值必须使用自己的constexpr构造函数；
  * 必须使用默认的析构函数。


#### 注：    
1、聚合类型（Aggregate Class）是指满足以下条件的类型：

* 所有的数据成员都是public，
* 没有用户定义的任何构造函数，
* 没有类内初始化in-class initializer，
* 没有基类和virtual函数的类型

2、constexpr构造函数：

* 函数体必须为空；
* 可以被声明为 =default；
* 必须初始化所有的数据成员；

{% highlight c++ %}
class Debug {
public:
     constexpr Debug(bool b) : hw(b) { }
};
{% endhighlight %}

