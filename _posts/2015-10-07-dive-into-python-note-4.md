---
layout: page
title: "dive-into-python-笔记-Ⅳ"
comments: true
tags: [python]
categories: python
header:
   image_fullwidth: "header_unsplash_library.jpg"
---
# 学习目标
1. 使用import module或from module import导入模块
2. 定义和实例化类
3. 定义\__init__和其他专用类方法
4. 子类化UserDict来定义行为像字典的类
5. 定义数据属性和类属性，并理解它们之间的不同
6. 定义私有属性和方法

# 本章内容    
##示例

```python

# -*- coding: cp936 -*-
#fileinfo

import os
import sys #导入模块的方法1，使用时要用sys做限定
from UserDict import UserDict#导入模块方法2，可直接使用

def stripnulls(data):
    '''strip whitespace and nulls'''
    return data.replace('\00','').strip()

class FileInfo(UserDict):#定义FileInfo类，并继承自UserDict
    '''store file metadata'''
    def __init__(self,filename=None):
        UserDict.__init__(self)#覆盖时，子类要显示调用父类的方法
        self['name']=filename

class Mp3FileInfo(FileInfo):#定义MP3FileInfo类并继承自FileInfo
    '''store mp3 tags'''
    tagDataMap={'title':(3,33,stripnulls),
                'artist':(33,63,stripnulls),
                'album':(63,93,stripnulls),
                'comment':(97,126,stripnulls),
                'genre':(127,128,ord)
        }#定义类的属性，其相当于C++的static数据成员

    def __parse(self,filename):#定义私有方法，通过方法名来识别
        'parse tags from mp3file'''
        self.clear()
        try:
            fsock=open(filename,'rb',0)
            try:
                fsock.seek(-128,2)
                tagdata=fsock.read(128)
            finally:
                fsock.close()
            if tagdata[:3]=='TAG':
                for tag,(start,end,parseFunc) in self.tagDataMap.items():
                    self[tag]=parseFunc(tagdata[start:end])
        except IOError:
            pass

    def __setitem__(self,key,item):#类的专有方法，可以不显示调用
        if key=='name' and item:
            self.__parse(item)
            FileInfo.__setitem__(self,key,item)#同样的，子类覆盖父类方法需要显示调用父类同名方法

def listDirectory(directory,fileExtList):
    '''get list of file info objects for files of particular extensions'''
    fileList=[os.path.normcase(f) for f in os.listdir(directory)]
    fileList=[os.path.join(directory,f) \
                for f in fileList \
                if os.path.splitext(f)[1] in fileExtList]
    def getFileInfoClass(filename,module=sys.modules[FileInfo.__module__]):
        '''get file info class from filename extension'''
        subclass="%sFileInfo" % os.path.splitext(filename)[1].upper()[1:]
        return hasattr(module,subclass) and getattr(module,subclass) or FileInfo
    return [getFileInfoClass(f)(f) for f in fileList]

if __name__=="__main__":
    for info in listDirectory("G:\\store\\music",[".mp3"]):
        print "\n".join(["%s=%s" %(k,v) for k,v in info.items()])
        print     
```

## 导入模块的两种方法
方法1：import module
方法2：from module import ...
区别：方法2直接导入到局部空间，使用时无需加上限定名


## 如何定义类
###最简单的python类

```python

class Base:#类的名字是Base，以class关键字开始，最后面跟一个冒号
	pass   #该类没有定义任何方法和属性，pass是一条什么都不做的语句
```

###继承一个已有类

```python

class Base:
	pass  

class Derived(Base):#Derived是子类，继承自Base基类
	pass

class Derived1():
	pass
#多继承写法，Derived2继承自Derived和Derived1基类
class Derived2(Derived1,Derived):
	pass
```

##如何初始化
+ \__init__在类实例化之后被调用，其作用类似于构造函数但并不是构造函数，因为\__init__是在对象被构造之后被调用的
+ \__init__的第一个参数是self，意思是指向类当前实例的引用；其行为类似于C++中的this指针
+ 在python中，所有类的方法第一个参数都是指向当前实例的引用，传递参数时无需指定，python会自动指定；按照惯例，第一个参数通常命名为self
+ 无论何时子类想要扩展父类行为，必须在适当时机，使用适当参数，必须**显示调用**父类方法

```python

#模块导入到局部空间
from UserDict import UserDict 

#FileInfo继承自UserDict
class FileInfo(UserDict):
    '''store file metadata'''
    #第一个参数总是self，可使用缺省参数
    def __init__(self,filename=None):
	    #必须显示调用在父类中的合适方法，注意这里传递了self
        UserDict.__init__(self)
        self['name']=filename
```

##如何实例化

+ 要对类进行实例化，只要调用类，**就好像是一个函数一样**，传入定义在\__init__方法中的参数，返回值将是一个新创建的对象 
+ 通常，不需要明确释放一个类的示例，当对象超过作用域时会被自动释放；python的底层实现是通过引用计数来正确处理对象销毁

```python

>>> import fileinfo
>>> file1=fileinfo.FileInfo("G:\\Tmp\\fileinfo.py")#构造一个实例，在内部会调用__init__
>>> file1.__class__#__class__是所有类的一个内置属性
<class fileinfo.FileInfo at 0x0257A880>
>>> file1.__doc__#这正是我们自己写的doc string
'store file metadata'
>>> file1
{'name': 'G:\\Tmp\\fileinfo.py'}
```   


##类的方法

###专用类方法

专用方法是在特殊情况下由python替你调用，而不是在代码中直接调用；它提供了一种方法，可以将非方法调用语法映射到方法调用上   

+ 任何类可以像字典一样保存键-值对只要定义 \__setitem__方法
+ 任何类可以表现的像一个序列，只要定义\__getitem__方法
+ 任何定义了\__cmp__方法的类可以使用==
+ 任何定义了\__len__方法的类可以调用len(instance)计算长度
+ \__call__方法让一个类表现的像一个函数，可以直接调用一个类的实例

####获得和设置数据项

```python

def __getitem__(self, key):return self.data[key]
def __setitem__(self, key, item):self.data[key]=item

#以下两种方式等价
>>> file1.__getitem__('name')
'G:\\Tmp\\fileinfo.py'
>>> file1['name']
'G:\\Tmp\\fileinfo.py'

```

### 私有方法

在python中一个函数，方法或属性是私有还是公有完全取决于它的名字。如果一个python函数，方法或属性以两个下划线开始，但不以两个下划线结束，那么它是私有的；其他情况都是公有的   
python中没有保护(protected)的概念   
如上所见，\__parse是私有方法，\__setitem__是一个专有方法   

```python

	#私有方法的定义，通过方法名来识别是否是私有方法
    def __parse(self,filename):
```

## 类的属性   

+ 类的属性可分为：类属性和数据属性
+ 类属性属于整个类，定义在类中；可以通过直接对类的引用，也可以通过任意类的实例来使用；如示例中tagDataMap。类似于C++中static成员
+ 数据属性类似于C++中普通数据成员，定义在\__init__中

