---
layout: post
category: C++学习笔记
title: C++中的字面值常量（Literal）
tags: C++
---

只有内置类型存在字面值，没有类类型的字面值。

## 一、整型字面量
* 整型字面量可以用十进制、八进制和十六进制来表示。如20可以用以下方式表示：
{% highlight c %}
20     // decimal
024    // octal
0x14     // hexadecimal
{% endhighlight %}
* 默认地，十进制的字面量都是signed类型，而八进制和十六进制可以是signed和unsigned类型。
* 默认地，十进制字面量可以是int, long, long long，由值大小决定；八进制和十六进制可以是int, unsigned, long, unsigned long, long long, unsigned long long，由值大小决定。没有short类型的字面量。
* 要改变以上的默认值，我们可以使用后缀。
{% highlight c %}
24L      // long类型
42ULL     // unsigned long long类型
{% endhighlight %}
* 从技术上讲，十进制字面量永远不会表示负数。"-42"的含义是，"42"是字面量，"-"是取负值的操作符。

## 二、浮点型字面量
*  浮点型字面量可以用十进制和科学计数符号表示：  
{% highlight c %}
3.14159     // 十进制
3.14159E0     // 科学计数，也可以3.14159e0
{% endhighlight %}
* 浮点型字面量默认是double类型。

## 三、字符和字符串字面量

{% highlight c %}
'a'     //字符字面量, 默认是char，L'a'表示wchar_t类型
"a"     // 字符串字面量->"a\0"（这是为了兼容C语言）
L"Hello world!"     // 宽字符串
"Hello "
"World!"     //multiline string literal
{% endhighlight %}


## 四、布尔类型和指针类型字面量
* true
* false
* nullptr

## 五、非打印字符的转义序列

### C++中的转义字符：

|换行符     |newline     |\n|
|水平制表符  |   horizontal tab |    \t|
|纵向制表符  |   vertical tab    | \v|
|退格符     |backspace     |\b|
|回车符    | carriage return |    \r|
|进纸符     |formfeed     |\f|
|报警（响铃）符 |    alert（bell） |    \a|
|反斜线     |backslash     |\\|
|疑问号     |question mark  |   \?|
|单引号     |single quote   |  \'|
|双引号     |double quote   |  \"|

### 通用转义字符：可以将任何字符用通用转义字符来表示。
* 八进制表示形式：
{% highlight c %}
\o
\oo
\ooo
{% endhighlight %}
最多三个八进制数。

* 十六进制表示形式:
{% highlight c %}
\xh
\xhhh...
{% endhighlight %}
一个或多个十六进制数字。

* 举例：（ASCII码字符集)
{% highlight c %}
\7 (bell)
\12(newline)
\40(blanck)
\0(null)
\115('M')
\x4d('M')
{% endhighlight %}


## 附表：（字面量类型限定符）

|*前缀* |*含义* |*类型*|
|u|Unicode 16字符|char16_t|
|U |Unicode 32字符|char32_t|
|L|宽字符 |wchar_t|
|u8|utf-8(仅限字符串字面量)|char|  



|*后缀*  |   *最小类型*|
|u or U   |  unsigned     |
|l or L   |  long|
|ll or LL   |  long long|  



|*后缀* |   *类型*|
|f or F  |   float|
|l or L    | long double|

### 前后缀使用实例
{% highlight c %}
L'a'     // wide character
u8"Hi!"      // utf-8 string
128u     // unsigned
1024UL     // unsigned long
1L     // long
8Lu     // unsigned long
1E-3F     // float
{% endhighlight %}