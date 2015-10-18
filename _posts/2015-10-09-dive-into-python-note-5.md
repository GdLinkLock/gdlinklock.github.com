---
layout: page
title: "dive-into-python-笔记-Ⅴ"
comments: true
tags: [python]
categories: python
header:
   image_fullwidth: "header_unsplash_library.jpg"
---
# 学习目标
1. 使用try...except来捕获异常
2. 使用try...finally来保护额外的资源
3. 读取文件
4. 使用os模块来满足跨平台的需要
5. 通过将类看成对象并传入参数，动态地实例化未知类型的类

# 本章内容    

##异常处理   
+ 关键字：try except raise finally
+ 在python中很多地方使用了异常，可以用异常处理错误
+ 一个try except可以有一个else，如果try中没有异常，那么else子句会被执行

```python

# exception.py

#自定义一个异常类，继承自Exception
class PiException(Exception):
    '''define a self exception for test'''
    def __init__(self,desc):
        self.desc=desc

    def __str__(self):
        return repr(self.desc)

def Fun():
    '''  '''
    try:
        print 'there is no exception in out try'
        try:
            print 'I will raise a self exception in inner try'
            raise PiException('this is a self exception')
        except PiException,e:
            print e
        else:
            print 'No!,I will never come here!'
    except :
        print 'I can catch all exceptions!'
    else:
        print 'Else,because there is no exception be catched'
    finally:
        print 'I always here wait for you!'

if __name__=='__main__':
    Fun()

```
###支持特定平台功能

```python 

#Bind the name getpass to appropriate function
try:
    import termios,TERMIOS#首先尝试导入unix模块
except ImportError:
    try:
        import msvcrt #如果失败则导入windows模块
    except ImportError:
        try:
            from EasyDialogs import AskPassword#如果既不是unix也不是windows则尝试导入ios模块
        except ImportError:
            getpass=default_getpass#如果不是以上三种平台则使用默认函数
        else:
            getpass=AskPassword#如果是ios平台
    else:
        getpass=win_getpass#如果是windows平台
else:
    getpass=unix_getpass#如果是unix平台
```

##文件对象   
+ open用来打开磁盘上的文件，open返回一个对象，它拥有一些方法和属性
+ f=open(fn,'r'),tell(),seek(),read(),closed,close(),write()
+ 文件读取，关闭，处理IO错误，写入文件

```python

#fileprocess.py

def FunRead(filename):
    dic={}
    #打开一个文件以rb方式，默认为r
    pf=open(filename,'rb');
    dic['filename']=pf.name
    #游标移动到文件尾
    pf.seek(0,2)
    #读取文件大小
    filesize=pf.tell()
    dic['filesize']=filesize
    #游标移动到文件开始
    pf.seek(0,0)
    #读取文件的前128字节
    readData=pf.read(128)
    dic['readData']=readData
    #关闭文件
    pf.close()
    dic['pfState']=pf.closed
    
    print ';'.join(['%s=%s' % (k,v) for k,v in dic.items()])

def FunWrite(filename):
	#打开文件，w表示写，a表示追加
    pf=open(filename,'w')#or pf=open(filename,'a')
    str='''
        一切都是命运
        一切都是烟云
        一切都是没有结局的开始
        一切都是稍纵即逝的追寻
        一切欢乐都没有微笑
        一切苦难都没有泪痕
        一切语言都是重复
        一切交往都是初逢
        一切爱情都在心里
        一切往事都在梦中
        一切希望都带着注释
        一切信仰都带着呻吟
        一切爆发都有片刻的宁静
        一切死亡都有冗长的回声'''
    #写文件
    pf.write(str)
    #依旧要关闭文件
    pf.close()

if __name__=='__main__':
    FunWrite("G:\\Tmp\\pi.txt")
    FunRead("G:\\Tmp\\pi.txt")
```
###os和sys的一些使用

+ os.environ 是在你的系统上所定义的环境变量的 dictionary
+ sys包含了系统及信息，如python版本(sys.version，sys.version_info)和系统及选项
+ sys.modules是一个字典，包含了从python运行开始起，被导入的所有模块；key是模块名，value是模块对象
+ 每个python类都有一个内置的类属性\__module__，它定义了这个类所属模块的名字；与sys.modules复合使用可得到模块的引用
+ os.path是一个模块的引用

```python 

>>> import sys
>>> print '\n'.join(sys.modules.keys())
heapq
code
......
>>> import os
#文件路径和文件名拼接
>>> os.path.join('G:\\tmp\\','pi.txt')
'G:\\tmp\\pi.txt'
#同上，os.path.join会自动在文件路径中添加\\
>>> os.path.join('G:\\tmp','pi.txt')
'G:\\tmp\\pi.txt'
#os.path.expanduser可得到当前用户根目录
>>> os.path.expanduser('~')
'C:\\Users\\lenovo'
#路径分割，返回一个tuple，包含文件路径和文件名
>>> (filepath,filename)=os.path.split('G:\\tmp\\pi.txt')
>>> filepath
'G:\\tmp'
>>> filename
'pi.txt'
#文件名分割，可以得到文件名和后缀
>>> (shortname,extension)=os.path.splitext(filename)
>>> shortname
'pi'
>>> extension
'.txt'
#得到某个目录下的所有子文件和子目录
>>> os.listdir('G:\\tmp')
['apihelper.py', 'cmake']
#os.path.isfile判断是否是文件
>>> os.path.isfile('G:\\tmp\\pi.txt')
True
>>> os.path.isfile('G:\\tmp\\')
False
#os.path.isdir判断是否是目录
>>> os.path.isdir('G:\\tmp\\')
True
#os.getcwd可得到当前工作目录
>>> os.getcwd()
'G:\\Tmp'
#文件路径格式统一为小写
>>> os.path.normcase('G:\\Tmp')
'g:\\tmp'
#glob模块可以使用通配符
>>> import glob
>>> glob.glob('G:\\tmp\\*.txt')
['G:\\tmp\\pi.txt', 'G:\\tmp\\poem.txt']
```

#小结  
1. 异常处理
2. 文件读写
3. 目录操作


