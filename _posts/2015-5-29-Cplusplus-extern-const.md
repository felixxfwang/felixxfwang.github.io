---
layout: post
category: C++学习笔记
title: C++中关于extern const的问题
tags: 
- C++
---

>转载自：  
[C++ - extern const的问题](ttp://blog.sina.com.cn/s/blog_76fbd24d0101a62k.html)

### 1. 具体问题场景

需要在A.cpp中定义一个const常量，并在B.cpp中引用这个常量，出错的场景：

{% highlight c++ %}
// A.cpp
const int NUM = 100;

// B.cpp
extern const int NUM;
{% endhighlight %}

编译错误，错误提示外部符号NUM无法解析，未定义。

### 2. 出错原因

const对象默认为文件的局部变量。

### 3. 关于非const

在全局作用域里定义非const变量时，它在整个程序中都可以访问。可以把一个非const变量定义在一个文件中，并在另外的文件中使用这个变量：

{% highlight c++ %}
// A.cpp
int counter;     // 定义非const变量counter

// B.cpp
extern int counter;  // 对A.cpp中的非const变量counter进行声明引用
++counter;        // 使用非const变量counter
{% endhighlight %}

### 4. 关于const

在全局作用域声明的const变量是定义在该对象的文件的局部变量。此变量只存在于那个文件中，不能被其他文件访问。通过显式地指定const变量为extern，就可以在整个程序中访问const对象。

{% highlight c++ %}
// A.cpp
extern const int NUM = 100;

// B.cpp
extern const int NUM;
for (i = 0; i < NUM; i++)
{% endhighlight%}

注意：B.cpp中的NUM也是extern，但是这个extern标志着NUM是一个声明，所以没有初始化式。

### 5. 列举几种情形的对与错
（1）ERROR: B.cpp中的NUM无法解析，未定义。

{% highlight c++ %}
// A.cpp
const int NUM = 100;

// B.cpp
extern const int NUM;
int i = NUM;
{% endhighlight %}

（2）OK

{% highlight c++ %}
// A.cpp
const int NUM = 100;

// B.cpp
extern const int NUM = 99;
int i = NUM;
{% endhighlight %}

（3）OK

{% highlight c++ %}
// A.cpp
extern const int NUM = 100;

// B.cpp
extern const int NUM;
int i = NUM;
{% endhighlight %}

（4）ERROR: NUM重定义

{% highlight c++ %}
// A.cpp
extern const int NUM = 100;

// B.cpp
extern const int NUM = 99;
int i = NUM;
{% endhighlight %}

（5）OK

{% highlight c++ %}
// A.cpp
extern const int NUM = 100;

// B.cpp
const int NUM = 99;
int i = NUM;
{% endhighlight %}

（6）OK

{% highlight c++ %}
// A.cpp
const int NUM = 100;

// B.cpp
extern const int NUM = 99;
int i = NUM;
{% endhighlight %}

### 6. 几种声明使用常量的方法

（1）在一个.cpp文件中使用extern定义const常量，在使用的地方extern声明引用。

（2）全部在头文件中进行const定义，在使用的地方包含头文件，但不要使用extern，否则多个包含此头文件的.cpp文件将出现重定义错误。

（3）在一个.cpp文件中extern定义，在头文件中extern声明，在使用的地方包含头文件（适用于大量常量情形）。

（4）避免常量，使用函数：

{% highlight c++ %}
double PI()
{
   return 3.1415926;
}
{% endhighlight %}



