---
layout: page
title: "虚拟内存"
comments: true
tags: [windows os]
categories: windows
header:
   image_fullwidth: "header_unsplash_library.jpg"
---
> 计算机没有魔法

# x86 virtual address space layouts

空间 | 用途 | 大小
---|--- |---
0x00000000 - 0x0000FFFF | NULL指针分区 | 64KB
0x00010000 - 0x7FFEFFFF | 进程可用分区 | 2G-64KB-64KB
0x7FFF0000 - 0x7FFFFFFF | 禁入分区 |  64KB
0x80000000 - 0xFFFFFFFF | 内核区域 | 2G

其中，Page Tables存放在0XC00000000 - 0XC0400000

# 32bit-nopae mode address translation

* 关键角色 
  - MMU(Memory Management Unit)
* 流程
  - 从CR3寄存器读取Page Directory
  - 取地址的高10bit通过查找Page Directory中的PDE(Page Directory Entry)可得到Page Table所在位置
  - 取地址中间10bit通过查找Page Table中的PTE(Page Table Entry)可以得到Page所在位置
  - 取地址的低12bit通过查找Page 得到物理地址
 
![image](/images/2016/20161120-lineraddress-to-physical.png)

* 简单示意图

![image](/images/2016/20161120-virtual-translation-physical.png)

# 几个易混淆的词汇

* Commited
  - is not all allocated memory, but the process’ “accessible” memory
  - 要点是not all allocated ，还有一部分是reverseed memory
* private bytes
  - memory not shareable with other processes
  - 要点是非share，为该进程独有
* working set
  - virtual memory that is resident in physical RAM
  - 要点是Ram，另外的两种back是page file和map file

