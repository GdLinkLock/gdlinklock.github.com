---
layout: page
title: "stream copy"
comments: true
tags: [C++ stream]
categories: C++ 
header:
   image_fullwidth: "header_unsplash_library.jpg"
---

# 问题

如何将一个stream中的数据传递到另一个stream

# 思路

将stream转成iterator，然后使用std::copy算法

# 实现

~~~cpp

// ConsoleApplication2.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <sstream>
#include <fstream>
#include <iostream>
#include <iterator>
#include <algorithm>
int main()
{
	//stringstream 
	std::stringstream ss;
	ss << "hello,this is characters from stringstream!" << std::endl;

	std::ofstream fs;
	fs.open("1.txt");

	std::copy(std::istreambuf_iterator<char>(ss),
		std::istreambuf_iterator<char>(),
		std::ostream_iterator<char>(fs));

	getchar();
    return 0;
}



~~~