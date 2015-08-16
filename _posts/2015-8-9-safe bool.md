---
layout: post
title: "safe bool的简单用法"
date: 2015-08-09 00:04
comments: true
categories: 
---

<!-- more -->


#safe bool

> 解决问题：类型重载所带来的诡异表达式
> 原理：类型重载
> 好处：编译期发现不合适的转型

```
class Test1
{
public:
	bool IsValid(){return true;}
};

class Test2
{
public:
	operator bool(){return true;}
};

class Test3
{
public:
	operator void*(){return this;}
};

class Test4
{
private:
	struct dummy   {void nonnull() {};};
	typedef void (dummy::*safe_bool)();
	const bool empty() const {return false;}
public:
	operator safe_bool () const
	{
		return (this->empty())? 0 : &dummy::nonnull; 
	}
};
int main()
{
	Test1 t1;
	if(t1.IsValid())
	{
		//需要显示调用IsValid
	}

	Test2 t2;
	if(t2)
	{
		int tmpVal=t2;//重载运算符，将一个对象赋值给一个int，什么行为？
	}

	Test3 t3;
	if(t3)
	{
		delete t3;//重载void*，将一个对象delete，什么行为？
	}

	Test4 t4;
	if(t4)
	{
		int tmpVal=t4;//error，编译期报错
		delete t4;//error，编译期报错
	}
	return 0;
}
```
