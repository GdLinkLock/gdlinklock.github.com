---
layout: page
title: "compiletime assert"
comments: true
tags: [静态断言 C++ assert]
categories: boost
header:
   image_fullwidth: "header_unsplash_library.jpg"
---
# 静态断言
我们都用过assert也就是运行时断言检查，这段时间看到几个有趣的静态断言，汇总一下放在这里。

## 方式1
- 原理：除0会导致编译器报错
- 实现方式

~~~cpp

#define static_assert(e)\
    do{\
        enum{value=1/(e)};\
    }while(0)
	
~~~

## 方式2
- 原理：分配大小为0的数组会导致编译器报错
- 实现

~~~cpp
#define static_check(e){char unnanmed[(e)]?1:0];}

~~~

## 方式3
- 原理:模板特化
- 实现

~~~cpp

template<bool> struct CompileTimeError;

template<> struct CompileTimeError<true>{};

#define static_check(e) (CompileTimeError<(e)!=0>())

~~~

## 方式4 
- 原理:模板特化
- 实现

~~~cpp

template<int> struct CompileTimeError;
template<> struct CompileTimeError<true> {};

#define static_check(e, msg) \
    { CompileTimeError<((e) != 0)> ERROR_##msg; \  
         (void)ERROR_##msg; } 
~~~

## 方式5
- 原理：C++11中内置断言
- 使用
  - 参数1：一个断言表达式，返回bool数值
  - 参数2：一个警告信息，通常是一段字符串

~~~cpp

static_assert(expr,"info");

~~~

## 小结
起初```c++```并没有原生的静态断言，使用者不得不自己利用语言本身的特性去实现一个静态断言。为了让静态断言更加方便使用，Andrei大牛在Loki库中实现了一个可以定制错误信息的断言。而在C++11中用于终于有了内置的静态断言。