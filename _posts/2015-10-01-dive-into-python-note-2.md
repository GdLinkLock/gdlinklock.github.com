---
layout: page
title: "dive-into-python-笔记-Ⅱ"
comments: true
tags: [python]
categories: python
header:
   image_fullwidth: "header_unsplash_library.jpg"
---

# 学习目标
1. 定义dictionary、list、tuple
2. 任意一个对象的访问方法，包括字符串、list、函数、模块
3. 通过字符串格式化连接值
4. 使用list解析 映射list为其他list
5. 把字符串分隔为list和把list链接为字符串

# 本章内容    
## Dictionary  
Dictionary 是 Python 的内置数据类型之一，它定义了键和值之间一对一的关   

```python 

>>> mydic={"name":"link","age":"18","profession":"se"}#定义dictionary,使用{}
>>> mydic
{'age': '18', 'profession': 'se', 'name': 'link'}
>>> mydic["age"] #访问存在的key
'18'
>>> mydic["myage"]#访问不存在的key，抛出异常

Traceback (most recent call last):
  File "<pyshell#6>", line 1, in <module>
    mydic["myage"]
KeyError: 'myage'
>>> mydic["address"]="beijing" #添加新元素
>>> mydic
{'age': '18', 'profession': 'se', 'name': 'link', 'address': 'beijing'}
>>> mydic[42]="halo"#添加新元素，key为整数
>>> mydic
{'age': '18', 42: 'halo', 'profession': 'se', 'name': 'link', 'address': 'beijing'}
>>> del mydic[42] #删除某个元素
>>> mydic
{'age': '18', 'profession': 'se', 'name': 'link', 'address': 'beijing'}
>>> mydic.clear() #删除所有元素
>>> mydic
{}
```
**注意**  

+ Dictionary元素没有顺序的概念，它们只是序偶的简单排列
+ Dictionary中的key大小写敏感
+ key可以为字符串、整数或者tuple
+ value可以是任意数据类型

## List    
List可以保存任意对象，且在增加新元素时动态扩展

```python

>>> mylist=["hello","world","good"] #定义list,使用[]
>>> mylist
['hello', 'world', 'good']
>>> mylist[0] #使用索引值访问一个存在的值
'hello'
>>> mylist[5]#访问一个不存在的值会抛出异常

Traceback (most recent call last):
  File "<pyshell#18>", line 1, in <module>
    mylist[5]
IndexError: list index out of range
>>> mylist[-1] #使用负的索引，list[-n]=list[len(list)-n];这里list[-1]=list[2]
'good'
>>> mylist[-6]#使用负的索引访问一个不存在的值

Traceback (most recent call last):
  File "<pyshell#20>", line 1, in <module>
    mylist[-6]
IndexError: list index out of range
>>> mylist[1:2] # 分片，结果是返回一个新的list；前闭后开[)
['world']
>>> mylist[0:2]
['hello', 'world']
>>> mylist[1:5]
['world', 'good']
>>> mylist[-3:2]#分片时超出索引范围不会抛出异常
['hello', 'world']
>>> mylist.append("new")#追加一个元素
>>> mylist
['hello', 'world', 'good', 'new']
>>> mylist.insert(2,"new")#在指定索引插入一个元素
>>> mylist
['hello', 'world', 'new', 'good', 'new']
>>> mylist.extend(["two","element"])#在末尾插入新的list
>>> mylist
['hello', 'world', 'new', 'good', 'new', 'two', 'element']
>>> mylist[-1]
'element'
>>> mylist.index("new")#查询一个元素
2
>>> 'hello' in mylist#简单的检查一个元素是否存在
True
>>> mylist.index('halo')#查询一个不存在的元素，抛出异常

Traceback (most recent call last):
  File "<pyshell#35>", line 1, in <module>
    mylist.index('halo')
ValueError: 'halo' is not in list
>>> mylist.remove('new')#删除一个元素
>>> mylist
['hello', 'world', 'good', 'new', 'two', 'element']
>>> mylist.pop()#删除最后一个元素，并返回被删除的元素
'element'
>>> mylist
['hello', 'world', 'good', 'new', 'two']
>>> mylist +=["third"]#+=操作符相当于链接，功能类似extend
>>> mylist
['hello', 'world', 'good', 'new', 'two', 'third']
>>> mylist *=2#*操作符相当于重复器
>>> mylist
['hello', 'world', 'good', 'new', 'two', 'third', 'hello', 'world', 'good', 'new', 'two', 'third']
```
**注意**  

+ List中的元素是有序的
+ append参数可以是任意类型，extend参数只能是一个list


## Tuple     

Tuple是不可变的List，一旦创建了一个Tupel，就不能以任何方式改变它   

```python

>>> mytuple=("hello","world")#定义个tuple，使用()
>>> mytuple
('hello', 'world')
>>> mytuple[0:2]#tuple支持分片
('hello', 'world')
>>> mytyple.index("hello")

Traceback (most recent call last):
  File "<pyshell#47>", line 1, in <module>
    mytyple.index("hello")
NameError: name 'mytyple' is not defined
>>> mylist=mytuple#使用tuple初始化list
>>> mylits

Traceback (most recent call last):
  File "<pyshell#49>", line 1, in <module>
    mylits
NameError: name 'mylits' is not defined
>>> mylist
('hello', 'world')
>>> 
```
**注意**  

+ Tuple中的元素不能改变，只能使用in关键字查看是否存在
+ Tuple和List可以相互转换，内存的tuple函数接受一个list并返回一个tuple，内置的list函数可以接受一个tuple并返回一个list；从效果上看，tuple冻结一个list，而list解冻了tuple




## 变量声明     
在python中也有全局变量和局部变量之分，但是python没有明显的变量声明，变量通过首次赋值产生，当超出作用范围时自动消亡
引用未初始化变量
一次赋多值
连续赋多值


## 格式化字符串     
基本格式：字符串表达式  % 元组/单个数据     

```python

>>> "myname is %s,myage is %d,i have %f rmb :-)" % ('link',18,55.456)
'myname is link,myage is 18,i have 55.456000 rmb :-)'
```
## 映射字符串     
Python 的强大特性之一是其对 list 的解析，它提供一种紧凑的方法，可以通过对 list 中的每个元素应用一个函数，从而将一个 list 映射为另一个 list

```python

>>> li = [1, 9,8, 4]
>>> [elm*2 for elm in li]
[2, 18, 16, 8]
>>>   
```


##字符串分隔和链接      
  
```python

>>> mylist
('hello', 'world')
>>> s= ";".join(mylist)#字符串连接join
>>> s.split(";")#字符串分隔split
['hello', 'world']
```

## 小结

到目前为止，应该掌握了一下内容：  

1. 使用python IDE来交互式的测试表达式
2. 编写python程序，分别从IDE和命令行运行
3. 导入模块并调用该模块函数
4. 声明函数以及doc string、局部变量和缩进的使用
5. 定义dictionary、list、tuple
6. 任意一个对象的访问方法，包括字符串、list、函数、模块
7. 通过字符串格式化连接值
8. 使用list解析 映射list为其他list
9. 把字符串分隔为list和把list链接为字符串