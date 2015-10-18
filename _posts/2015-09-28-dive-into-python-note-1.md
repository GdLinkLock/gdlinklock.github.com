---
layout: page
title: "dive-into-python-笔记-Ⅰ"
comments: true
tags: [python]
categories: python
header:
   image_fullwidth: "header_unsplash_library.jpg"
---

#本章内容  
1. 函数如何定义和使用
2. 静态类型语言，动态类型语言，强类型语言，弱类型语言
3. 文档化函数
4. 万物皆对象
5. 代码缩进
6. 测试模块   

#开门见山：第一个示例程序
```python

#odblhelper.py
#函数声明
def buildConnectionString(params): 
	#文档化函数 doc string
    """Build a coonection string from a dictinary of parameters。

    Return string."""
    #join是string的一个方法；用于连接字符串
    #这里params是一个Dictionary，items()是其一个方法，for语句用来遍历Dictionary每个键值对
    #'%s=%s' %(k,v)用于格式化
    return ';'.join(['%s=%s' %(k,v) for k,v in params.items()])

#测试模块，如果是直接运行则执行以下内容否则跳过
if __name__=='__main__':
    myparams={'server':'mpligrim','db':'master','uid':'sa','psw':'sa'}
    #函数调用部分，并直接打印返回结果
    print buildConnectionString(myparams)

output:
>>> 
db=master;uid=sa;psw=sa;server=mpligrim
``` 

#函数声明  
def buildConnectionString(params): 

    """doc string"""
    函数体
    
+ 函数声明以关键字 def 开始，接着为函数名，再往后为参数，参数放在小括号里。多个参数之间 (这里没有演示)用逗号分隔
+ 每个 Python 函数都返回一个值；如果函数执行过 return 语句，它将返回指定的值，否则将返回 None(Python 的空值)
+ 在 Python 中参数，params 不需要指定数据类型。Python会判定一个变量是什么类型，并在内部将其记录下来

#编程语言比较
+ 静态类型语言
一种在编译期间就确定类型的语言；大多数静态语言是通过要求在使用任一变量之前声明其数据类型来保证这一点的。java和c都是静态类型语言
+ 动态类型语言
一种在运行期才去确定数据类型的语言；python属于动态类型语言。动态类型语言确定一个变量的类型是在第一次给它赋值的时候
+ 强类型语言
一种总是强制类型定义的语言，java和python都是强制类型定义的。假如有一个整数，如果不进行明确的转换，不能把它当作一个字符串
+ 弱类型语言
一种类型可以被忽略的语言，比如在vb中可以将字符串‘12’和整数3进行连接得到字符串‘123’，然后把它看成整数123，所有这些都不需要任何转换

由此可见，python既是动态类型语言，又是强类型语言


#什么是文档化函数  
可以给出一个doc string来文档化函数；如下：

```python
def buildConnectionString(params): 
    """Build a coonection string from a dictinary of parameters。

    Return string."""
``` 
**表示：使用三引号**  
**约束：必须是函数定义的第一个内容**   
**必要性：相当于一个函数的说明，方便大家使用和维护 **  

#怎样理解万物皆对象  
在python中，“对象”的定义是松散的；有些对象既没有属性也没有方法，也不是所有的对象都可以实例化。万物皆对象从感性上可以理解为：一切都可以赋值给变量，或作为参数传递给函数
(似乎还是很模糊，接着往下看。。。)

#代码缩进  
python中代码块是通过缩进来定义的，开始缩进表示块的开始，取消缩进表示块的结束。不存在明显的大括号或者bengin，end之类的关键字，惟一的分隔符是冒号(:)，接着是代码本身的缩进

#测试模块  
所有python模块都是对象，并且有几个有用的属性，\__name__属性是其中之一
一个模块的name值取决于我们如何使用该模块；如果是import模块，那么\__name__的值通常为模块的文件名，不带路径或者文件扩展名；如果是直接运行该模块，那么\__name__值将是一个特别的确幸值\__main__。
因此可以通过\__name__来判断该模块是被导入的还是直接运行的，这样使得在将新的模块导入到一个大程序之前开发和调试容易的多

# 小结  
1. 对python有一个初步认识即可，接下来深入到python中去



