---
title: 使用livekd的步骤
date: 2019-02-14 08:31:15
tags: windows
typora-root-url: 2019-02-14-the-step-use-livekd
---

## Step1 

下载sysinternal

下载Windows SDK

## Step2

将livekd拷贝到windbg所在目录下

将windbg所在目录添加到系统环境变量中

## Step3

使用管理员权限打开cmd

livekd 

![1550104738494](/1550104738494.png)

livekd -w

![1550104825217](/1550104825217.png)



如果已经打开了一个内核调试，当再次打开时会遇到以下错误

![1550105092617](/1550105092617.png)