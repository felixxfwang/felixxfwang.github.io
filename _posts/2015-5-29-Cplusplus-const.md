---
layout: post
category: C++学习笔记
title: C++中的常量const
tags: 
- C++
---

常类型是指使用类型修饰符const修饰的类型，常类型的变量或对象的值是不能被更新的。  
const推出的初始目的，正是为了取代预编译指令，消除它的缺点，同时继承它的优点。


## 一、基本知识

const保证变量值的不变性。

{% highlight c++ %}
const int i = getSize(); // ok: 在运行时初始化（initialized at run time）
const int j = 42;        // ok: 在编译时初始化（initialized at compile time）
const int k;             // error: 必须初始化

int i = 42;
const int ci = i;     // ok, 将i的值复制给ci
int j = ci;           // ok, 将ci的值复制给j
{% endhighlight %}

1. 一个const类型的对象不可以做那些会改变这个对象的操作。
2. 编译器会在编译时将所有的const变量用它的值取代。
3. 与其他变量不同，在全局作用域声明的const对象默认为定义该对象的文件的局部变量。此变量只存在于那个文件中，不能被其他文件访问。
4. 非const变量默认是extern。 
如果想要在不同的文件里共享同一个const对象，需要在声明和定义中都使用extern关键字。如下：

{% highlight c++ %}
// file.h
extern const int bufSize; //bufSize和在file.cpp中定义的bufSize是同一个对象
// file.cpp
extern const int bufSize = fcn(); 
{% endhighlight %}

在上面的代码中，我们定义了bufSize的单例，这样就可以在其他文件中使用这个const对象了。

## 二、const和引用

{% highlight c++ %}
const int ci = 1024;
const int &r1 = ci;    // ok
r1 = 42;               // error,
int &r2 = ci;          // error, 非const引用不能引用一个const对象

int i = 42;
const int &r1 = i;      // ok
const int &r2 = 42;     // ok
const int &r3 = r1 * 2; // ok
int &r4 = r1 * 2;        // error

double dval = 3.14;
const int &ri = dval;   // ok
{% endhighlight %}

1、一般地，引用const对象的引用（reference to const） 和 const引用（const reference）是指一个东西。

2、const引用可以绑定一个非const对象。

{% highlight c++ %}
int i = 42;
const int &r1 = i;     // ok
{% endhighlight %}

但是必须明白，const引用只是限定了不能通过r1引用对i进行修改，但是i的值可以通过其他方式进行修改。

3、非const引用只能绑定到与该引用同类型的对象；
const引用则可以绑定到不同但相关类型的对象或绑定到右值（包括字面量，和范围更宽广的表达式）。

举个例子来理解“const引用可以绑定到不同但相关类型的对象或绑定到右值”。

{% highlight c++ %}
double dval = 3.14;
const int &ri = dval;     // ok
{% endhighlight %}

实际上，编译器做了这样一个工作：

{% highlight c++ %}
const int tmp = dval;     // 从dval创建了一个临时的tmp常量
const int &ri = tmp;     // 将临时的tmp常量绑定给ri
{% endhighlight %}

如果ri不是const，那么可以给ri赋一新值。这样做不会修改dval，而是修改了tmp。期望对ri的赋值会修改dval的程序员会发现dval的值并没有修改，所以非const引用不能绑定到其他类型的对象。仅允许const引用绑定到需要临时变量作为媒介来完成绑定过程的值，因为const引用是只读的。 


## 三、const和指针

{% highlight c++ %}
const double pi = 3.14;
double *ptr = &pi;          // error, 非const指针不能指向const对象
const double *cptr = &pi;   // ok
*cptr = 42;                 // error, *cptr不能进行赋值操作

double dval = 3.14;
const double *cptr = &dval;     // ok, 但是不能通过cptr改变dval的值

int errNumb = 0;
int *const curErr = &errNumb;     // curErr会永远指向errNumb对象
const double pi = 3.1415;
const double *const pip = &pi;     // pip是一个指向const对象的const指针
{% endhighlight %}

1、指向const对象的指针（pointer to const）可以指向一个非const对象，但是不能通过这个const指针来修改这个对象的值。

{% highlight c++ %}
double dval = 3.14;
const double *cptr = &dval;     // ok, 但是不能通过cptr改变dval的值
{% endhighlight %}

2、指向const对象的指针（pointer to const） vs const指针（const pointer）

{% highlight c++ %}
A const double *ptr = &val;     // 定义指向const对象的指针
B double *const ptr = &val;     // 定义const指针
{% endhighlight %}

  理解这两个定义：  
  从右往左读。  
  对于A定义，ptr左边是*，说明ptr是一个指针，剩下的const double是ptr的类型，所以A定义了指向const double对象的指针；  
  对于B定义，ptr左边是const，说明ptr是一个const对象，剩下的double *是ptr的类型（指向double类型的指针），所以B定义了指向double对象的const指针。

## 四、顶层const和底层const

顶层const（top-level const）：指针本身是一个const对象  
底层const（low-level const）：指针所指的对象是一个const对象

更一般的，
顶层const可以表示任意的对象是常量，这一点对任何数据类型都适用，如算术类型、类、指针等；
底层const则与指针和引用等复合类型的基本类型部分有关。
比较特殊的是，指针类型既可以是顶层const也可以是底层const，这一点和其他类型相比区别明显：

{% highlight c++ %}
int i = 0; 
int *const p1 = &i;         // 不能改变p1的值，这是一个顶层const 
const int ci = 42;      // 不能改变ci的值，这是一个顶层const 
const int *p2 = &ci;        // 允许改变p2的值，这是一个底层const 
const int *const p3 = p2; // 靠右的const是顶层const，靠左的是底层const 
const int &r = ci;       // 用于声明引用的const都是底层const
{% endhighlight %}

 当执行对象的拷贝操作时，常量是顶层const还是底层const区别明显。  
A、顶层const不受什么影响：

{% highlight c++ %}
i = ci;     // 正确：拷贝ci的值，ci是一个顶层const，对此操作无影响 
p2 = p3;    // 正确：p2和p3指向的对象类型相同，p3顶层const的部分不影响
{% endhighlight %}

执行拷贝操作并不会改变被拷贝对象的值。

B、底层const的限制却不能忽视。
当执行对象的拷贝操作时，

* 拷入和拷出的对象必须具有相同的底层const资格，
* 或者两个对象的数据类型必须能够转换（一般来说，非常量可以转换成常量，反之则不行）。

{% highlight c++ %}
int *p = p3;            // 错误：p3包含底层const的定义，而p没有 
p2 = p3;                // 正确：p2和p3都是底层const 
p2 = &i;                // 正确：int*能转换成const int* 
int &r = ci;            // 错误：普通的int&不能绑定到int常量上 
const int &r2 = i;      // 正确：const int&可以绑定到一个普通int上
{% endhighlight %}

p3既是顶层const也是底层const，拷贝p3时可以不在乎它是一个顶层const，但是必须清楚它指向的对象得是一个常量。因此，不能用p3去初始化p，因为p指向的是一个普通的（非常量）整数。另一方面，p3的值可以赋给p2，是因为这两个指针都是底层const，尽管p3同时也是一个常量指针（顶层const），仅就这次赋值而言不会有什么影响。
 
