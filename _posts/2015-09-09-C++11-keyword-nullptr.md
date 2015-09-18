---
layout: page
title: "C++11 nullptr"
comments: true
tags: [C++11]
categories: C++11
header:
   image_fullwidth: "header_unsplash_library.jpg"
---

# 什么是nullptr

nullptr是C++11中新增的一个关键字，是一个“指针空值类型”的常量；指针空值类型被命名为nullptr_t
那么nullptr_t又何许人也？我们来看看定义，这里以vs2012为例，在头文件&lt;stddef>中我们找到如下定义：

~~~cpp

#ifdef __cplusplus
namespace std { typedef decltype(__nullptr) nullptr_t; }
using ::std::nullptr_t;
#endif

~~~

等等，不是nullptr吗，怎么变成了\_\_nullptr？去查阅文档的话，会发现定义有所区别，文档中提到在头文件&lt;cstddef>中定义了nullptr(ps:猜测可能是vs自己做了处理,没有做相关跟踪)

~~~cpp

namespace std {typedef decltype(nullptr) nullptr_t; }

~~~

往常我们都是先定义类型，再通过类型来声明值，而这里一反常态，使用是另一个关键字decltype，关于decltype放到下次来学习

那么\_\_nullptr和nullptr又有什么区别呢？
>
>The C++/CLI language already had a nullptr keyword since 2005. That caused a problem when C++11 approved the nullptr keyword for C++. Now there are two, one for managed code and another for native code. The C++/CLI compiler can compile both. So you have to use __nullptr when you mean the native null pointer, nullptr when you mean the managed null pointer.

原来，在此之前nullptr关键字已经存在；使用\_\_nullptr是为了区分native code和manage code

# 为什么需要nullptr
##我们遇到了什么问题
在C++中，我们通常会用NULL来表示一个空指针；我们来看下面一段代码：

~~~cpp

void fun(char*ｃ)
{
	std::cout<<"char*ｃ"<<std::endl;
}
void fun(int i)
{
	std::cout<<"int i"<<std::endl;
}
int main()
{
	fun(0);				//int i
	fun(NULL);			//int i，其实我们想要访问指针的类型
	fun((char*)NULL);	//char*ｃ
	return 0;
}

~~~

示例中fun(NULL)预期访问fun(char*),但是其实不是。那么问题出在哪里？
先来看看NULL是如何定义的，在头文件&lt;stddef>中找到如下定义：

~~~cpp

#ifndef NULL
#ifdef __cplusplus
#define NULL    0
#else
#define NULL    ((void *)0)
#endif
#endif

~~~
可见，这里NULL的定义是字面常量0。在C++98中，字面常量0的类型既可以是一个整型，也可以是一个无类型指针void*.如果我们想要在代码中调用fun(char*)版本，那就必须对字面常量0进行转型；因为编译器总是优先把0当作字面常量。

##问题的根源是什么
字面常量0的二义性；既可以作为整型又可以作为无类型指针。
##如何解决
答案是nullptr；在C++11标准中，出于兼容性考虑并没有取消字面常量0的二义性，而是新增了一个关键字nullptr

# 什么时候使用nullptr
只要你的编译器支持，就尽量使用nullptr，减少NULL和0的使用
# 什么时候不可以用nullptr
当然，如果你的编译器不支持，你是不能使用的。如果你的程序需要移植到一些其他平台，而这些平台的编译器版本较低时，可能是个麻烦事。

# 关于nullptr与转换
C++11标准不仅定义了nullptr，也定义了其类型nullptr_t；也就表示了指针空值类型不仅仅nullptr一个实例

除去nullptr和nullptr_t，C++中还存在各种内置类型；C++11标准严格规定了数据间的关系：

+  所有定义为nullptr_t类型的数据都是等价的，行为完全一致
+  nullptr_t类型数据可以隐式转换成任意一个指针类型
+  nullptr_t类型数据不能转换成非指针类型，即使使用reinterpret_cast也是不可以的
+  nullptr_t类型数据不适用于算术运算表达式
+  nulllptr_t类型数据可以用于关系表达式，但仅能与nullptr_t类型数据或者指针类型数据进行比较，当且仅当运算符为==, <=, >=时返回true


# 小结

1. nullptr是C++11新增关键字，其类型是nullptr_t
2. 尽量使用nullptr_t替换NULL

#参考文献
1. 《深入理解C+++11》
2. [Revision of SC22/WG21/N2214 = J16/07-0074](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/)