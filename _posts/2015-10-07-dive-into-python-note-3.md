---
layout: page
title: "dive-into-python-笔记-Ⅲ"
comments: true
tags: [python]
categories: python
header:
   image_fullwidth: "header_unsplash_library.jpg"
---
# 学习目标
1. 用可选和命名参数定义和调用函数
2. 用str强制转换任意值为字符串
3. 用getattr动态得到函数和其他属性引用
4. 列表过滤
5. and-or使用
6. 定义lambda函数
7. 将函数赋值给变量，然后通过引用变量调用函数

# 本章内容    
## 示例

```python

#apihleper.py

#定义函数info，object是固定参数，spacing和collapse是缺省参数
def info(object,spacing=10,collapse=1):
    '''Print methods and doc strings.'''
    #if callable可以过滤列表
    methodList=[method for method in dir(object) if callable(getattr(object,method))]
    #相当于C++中的三元运算符 bool?a:b
    #split()缺省情况使用空白分隔字符串；
    processFunc=collapse and (lambda s:' '.join(s.split())) or (lambda s:s)
    #使用str是为了防止某些函数没有doc string的情况下发生异常
    #ljust格式化字符串，默认使用空格填充，可通过参数指定填充字符
    print '\n'.join(['%s %s' %
                     (method.ljust(spacing),
processFunc(str(getattr(object,method).__doc__)))
                     for method in methodList])

if __name__=='__main__':
    print info.__doc__
```
## 什么是自省  
自省是指代码可以查看内存中以对象形式存在的其它模块和函数，
获取它们的信息，并对它们进行操作。用这种方法，可以定义没有名称的函数，不按函数声明的参数顺序调用函数，甚至引用事先并不知道名称的函数


## 自省有什么用  

1. 可以定义没有名称的函数
2. 不必按函数声明的参数顺序调用函数


## 缺省参数和命名参数   

Python 允许函数参数有缺省值；如果调用函数时不使用参数，参数将获得它的缺省值。此外，通过使用命名参数还可以以任意顺序指定参数
***参数不过是一个字典而已***

## 基本内置函数   
###type函数   
type函数用于返回**任意对象**的数据类型

```python
>>> type(1)#整数
<type 'int'>
>>> type([])#列表
<type 'list'>
>>> type(apihelper)#模块
<type 'module'>
>>> import types
>>> type(apihelper)==types.ModuleType
True
#此外还有字符串，字典，元组，类，函数等
```

###str函数   

将任意数据类型的对象转换为字符串

```python

>>> apihelper
<module 'apihelper' from 'G:/Tmp\apihelper.py'>
>>> str(apihelper) #模块对象转换为字符串
"<module 'apihelper' from 'G:/Tmp\\apihelper.py'>"
>>> str(1)#整数转换为字符串
'1'
```

###dir函数   

dir函数返回任意对象的属性和方法列表

```python

>>> dir(apihelper)
['__builtins__', '__doc__', '__file__', '__name__', '__package__', 'info']
```
###callable函数  

callable可接受任何对象作为参数，如果参数是可调用的则返回True否则返回False；可调用对象包括：函数，类方法，甚至是类自身

```python

#string.punctuation是一个不可调的字符串对象，故返回false
>>> callable(string.punctuation)
False
#string.join是一个可调用的对象，故返回true
>>> callable(string.join)
True
```
###getattr函数   
getattr函数可以得到一个直到运行时才知道名称的函数的引用，也即是说可以返回任何对象的任何属性的一个引用

```python
>>> li=['one','two']
>>> getattr(li,'append')('three')#getattr返回一个引用，可直接调用
>>> li
['one', 'two', 'three']
```
##and-or-lambda   
+ and or 执行布尔逻辑运算，但不返回布尔值，而是返回实际进行比较的值之一 
+ python中的真与假：0 '' [] {} () None在布尔环境中为假，其他都为真
+ 如果布尔环境中所有值都为真，那么and返回最后一个真值；否则返回第一个假
+ 如果所有值都为假，则返回最后一个假值，否则返回第一个真
+ a and b or c当b为真时，相当于a==true?b:c；更严谨的写法是(a and [b] or [c])[0]
+ lambda参数列表没有括号，且忽略了return关键字，没有函数名单可以赋值给一个变量进行调用

# 小结   
自省是python的一个强大功能之一

