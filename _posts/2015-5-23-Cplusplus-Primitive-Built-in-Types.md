---
layout: post
category: [C++,学习笔记]
title: C++中的基本数据类型
tags: C++
---

最近开始重新学习C++，就把学习过程中遇到的一些重要的知识点记录下来，以后有遗忘了可以重新来翻阅。

C++中的基本类型包括：
1. 算术类型
2. void

# 一、存储空间
每种算术类型的存储空间是由编译器和目标机器决定的，c++标准只是规定了每个类型的minimum size, 如下：

| *算术类型* | *类型* | *含义* | *最小存储空间* |
| | bool | 布尔型 | NA |
| | char | 字符型：支持目标机器的基本字符集，占一个字节 | 8位 |
| | wchar_t | 宽字符型：支持目标机器的最大扩展字符集 | 16位 |
| | char16_t | Unicode字符：utf-16 | 16位 |
| 整型(integral types)| char32_t | Unicode字符：utf-32 | 32位 |
| | short | 短整型 | 16位 |
| | int | 整型 | 16位 |
| | long | 长整型 | 32位 |
| | long long | 长整型：C++11标准新增类型 | 64位 |
| | float | 单精度浮点型 | 6位有效数字 |
| 浮点型(floating-point types)| double | 双精度浮点型 | 10位有效数字 |
| | long double | 扩展精度浮点型 | 10位有效数字 |

1. C++标准要求int至少和short一样大， long至少和int一样大，long long至少和long一样大。例如，如果某台机器上int占了64位，那么long至少要64位，不能32位。
2. 通常来说，float占32位，double 64位，long double 96位或128位。float一般产生7位有效数字，double产生16位有效数字。

# 二、带符号和无符号类型（signed and unsigned）
1. signed型可以表示正数，0和负数；unsigned只能表示正数和0。
2. 除了布尔型和扩展字符型（wchar_t, char16_t, char32_t），其他的整型都可以分为带符号和无符号型。
3. short, int, long, long long都是带符号型，对应的无符号型是在前面加unsigned，如unsigned long。unsigned int 也可以简写为unsigned。
4. 字符型分为char，signed char，unsigned char三种，但是实际上只有signed cahr和unsigned char两种表示，char会使用这两种表示中的一种，具体使用哪一种由编译器决定。
5. unsigned类型所有的bits都用来表示数字，例如一个8位的unsigned char可以表示0-255；C++标准规定signed类型必须均分正数和负数，所以一个8位的signed char一定可以表示-127~127（很多现代机器也可以表示-128~127）。

# 三、类型使用的选择
1. 一般使用int。short太短；long通常和int有一样的size。如果需要更大的数值，则使用long long。
2. 不要用char来进行计算，如果需要一个很小的整数，使用signed char或者unsigned char。
3. 一般使用double。float精度太小，而且double计算的代价相对于float可以忽略，甚至比float还小。long double提供的精度通常没有必要，而且代价很高。

# 四、 类型转换

{% highlight c %}
unsigned char c = -1; // 假设char是8位的，则c=255  
signed char c2 = 256; // 假设char是8位的，则c is undefined  

unsigned u = 10;  
int i = -42;  
std:cout << u+i <<std:endl; // 如何是32位的int，则打印 4294967264  

unsigned u1 = 42, u2 =10;
std:cout << u2-u1 <<std:endl; // 如何是32位的int，则打印 4294967274  
{% endhighlight %}

1. 将超过范围的数值赋给unsigned的类型，编译器会将该值对unsigned类型的所有可能取值数目求模，然后取所得值。如上例中-1对2的8次方求模，得255。
2. 将超过范围的数值赋给signed类型，行为是不定的，由编译器决定。实际中很多编译器处理signed类型的方式和unsigned类型类似。
3. 至关重要的，unsigned和signed类型混合计算，signed的类型会自动转换为unsigned，然后进行计算，最后结果为unsigned类型。所有我们要避免unsigned和signed类型混合，以免产生意想不到的错误结果。

