---
layout: post
category: C++学习笔记
title: C++中的引用和指针初探
tags: 
- 指针
- 引用
- C++
---


C++中的复合类型是基于其他类型定义的类型。

C++中有几种复合类型，引用和指针是其中两种，此外数组也是一种复合类型。

## 一、引用（Reference）

使用举例：

{% highlight c++ %}
int ival = 23;
int &refint = ival;
refint = 2;
int iival = refint;
int &refint2 = refint;
{% endhighlight %}

1.定义引用的时候必须初始化，初始化的值必须是一个对象，不能是字面量。

2.一旦初始化，引用不能重新绑定其它对象,所以定义引用时必须初始化。

3.引用不是一个对象，引用是已经存在的对象的一个别名。

4.因为引用不是对象，所以不能使用引用来定义（初始化）另一个引用。

5.引用必须绑定一个对象，引用类型必须和绑定对象的类型一致（有两个例外），不能是字面量，也不能是范围更宽广的表达式。如：

{% highlight c++ %}
int &refint = 23;      //error

double dval = 3.14;
int &refVal = dval;     // error
{% endhighlight %}

例外1：  
const引用可绑定非const对象，字面量或者范围更宽广的表达式。

{% highlight c++ %}
const int &r2 = 42;     // ok

double dval = 3.14;
const int &ri = dval;     // ok
{% endhighlight %}

## 二、指针（Pointer）

1.指针不像引用，指针本身是一个对象。

2.同其他基本类型一样，块作用域内（block scope）定义且未被初始化的指针对象的值是未定义的（undefined）。

3.引用不是对象，所以引用没有地址，因此不能用引用来定义指针。

4.指针类型必须和指向对象的类型一致（有两个例外）。
     
例外1：可以使用指向const对象的指针（pointer to const）指向一个非const对象。

{% highlight c++ %}
double dval = 3.14;
const double *cptr = &dval;     // ok
{% endhighlight %}

5.指针的三种有效值：

- 指向一个对象；
- 指向某一对象后的另一对象（point to the location just immediately past the end of an object）；
- 指向一个空指针，表明未指向任何一个对象；

除了以上三种，其他都是无效值（invalid）。

6.获取空指针的三种方式：

{% highlight c++ %}
int *p = nullptr;     // 使用字面量nullptr
int *p1 = 0;
int *p2 = NULL;       // must #include cstdlib
{% endhighlight %}

nullptr是C++11标准引入的字面量，它是一种特殊的类型，可以转换为任何其他的指针类型。  
NULL是cstdlib中的预处理变量（preprocessor variable），NULL = 0。  
推荐使用nullptr而非NULL。  

7.void* 指针：  
void\*是一种特殊指针类型，它可以持有任何对象的地址，但是这个对象的类型是未知的。  
void\*指针只能做以下三件事：  

- compare with another pointer;
- pass it to or return it from a function;
- assign it to another void* pointer;

8.不能将int类型的变量赋值给指针，即使这个int变量为0，如：

{% highlight c++ %}
int i = 0;
int *p = i;      // error
{% endhighlight %}

9.建议：指针一定要初始化。

10.指针的引用（References to Pointers）

{% highlight c++ %}
int i = 42;
int *p;
int *&r = p;     // r是p的一个引用，别名
r = &i;
*r = 0;
{% endhighlight %}

对于int \*&r 这种声明，要从右往左看，最靠近r的&代表r是一个引用，左边剩下的代表r引用的对象的类型；
&r左边的\*代表r引用了一个指针，所以int \*&r代表r引用了一个int型的指针。

## 相关知识点

1.块作用域（block scope） —— 全局作用域（global scope）

2.&和\*的使用：

{% highlight c++ %}
int i = 42;
int &r = i;          // &是引用声明符号，r是一个引用
int *p;               // *是指针声明符号，p是一个指针
p = &i;               // &是取地址操作符（address of operator），在表达式中使用
*p = i;               // *是解引用操作符（dereference operator），在表达式中使用
int &r2 = *p;
{% endhighlight %}

3.预处理器是运行于编译器之前的。
在我们使用预处理变量的时候，预处理器会自动用它的值来替换它，然后编译器才进行编译。
