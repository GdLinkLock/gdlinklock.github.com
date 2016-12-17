---
layout: page
title: "设计模式-生成器模式"
comments: true
tags: [Design Pattern Builder]
categories: Design Pattern
header:
   image_fullwidth: "header_unsplash_library.jpg"
---

# 意图 

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示

# 适用性

* 当创建对象的复杂算法应该独立于该对象的组成部分以及它们的装配方式时
* 当构造过程必须构造的对象有不同的表示时

# 静态类图

![builder](/images/2016/20161217-Design-Pattern-Builder.jpg)

# 参与者

* Bilder
    * 为创建一个Product对象的各个部件指定抽象接口
* ConcreteBuilder
    * 实现Builder接口以构造和装配该产品的各个组件
    * 定义并明确它所创建的表示
    * 提供一个检索产品的接口
* Director
    * 构造一个使用Builder接口的对象
* Product
    * 表示被构造的复杂对象,ConcreteBuilder创建该产品的内部表示并定义它的装配过程
    * 包含定义组成部件的类，包括将这些部件装配成最终产品的接口 

# 协作

* 客户创建Director对象,并用他想要的Builder对象配置
* 一旦产品部件被生成，Director就会通知Builder
* Builder处理Director请求,并将部件添加到产品中
* 客户从Builder中检索产品

# 效果

* 它使你可以改变一个产品的内部表示
    * Builder提供一个抽象接口给Director
    * 该接口使得Builder可以隐藏Product的表示和内部结构，同时也隐藏了产品是如何装配的
    * 当需要改变产品的内部表示时，只需要定义一个新的Builder
* 它将构造代码和表示代码分开
    * Buildert通过封装一个复杂对象的创建和表示方式提高了对象的模块性。客户不需要知道定义产品结构的类的所有信息.
* 它使你可对构造过程进行更精细控制
    * Builder模式是在Director的控制下一步步创建创建产品的。仅当该产品创建完成时Director才从Builder中取走它。因此Builder模式相比其他创建型模式更能反映产品的创建过程，使得更容易控制产品的构建过程

# 实现

~~~ cpp
// 02builder.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <string>
#include <iostream>
#include <memory>
#include <vector>

constexpr int MOBIKE_HANDLERBAR_LENGTH = 25;//摩拜单车 车把长度
constexpr int OFOBIKE_HANDELERBAR_LENGTH = 30;//摩拜单车 车轮半径
constexpr int MOBIKE_WHELL_RADIUS = 40;//ofo单车 车把长度
constexpr int OFOBIKE_WHEEL_RADIUS = 43;//ofo单车 车轮半径
//抽象产品-车把
class Handlerbar
{
public:
	Handlerbar() {}
	~Handlerbar() {}
	//自行车车把长度
	virtual int GetLength() const { return 0; }
};
class MobikeHandlerbar :public Handlerbar
{
public:
	MobikeHandlerbar() {}
	~MobikeHandlerbar() {}
	virtual int GetLength() const { return MOBIKE_HANDLERBAR_LENGTH; }
};
class OfobikeHandlerbar :public Handlerbar
{
public:
	OfobikeHandlerbar() {}
	~OfobikeHandlerbar() {}
	virtual int GetLength() const { return OFOBIKE_HANDELERBAR_LENGTH; }
};
//抽象产品-车轮
class Wheel
{
public:
	Wheel() {}
	~Wheel() {}
	//自行车车轮半径
	virtual int GetRadius() const { return 0; }
};

class MobikeWheel :public Wheel
{
public:
	MobikeWheel() {}
	~MobikeWheel() {}
	virtual int GetRadius() const { return MOBIKE_WHELL_RADIUS; }
};
class OfobikeWheel :public Wheel
{
public:
	OfobikeWheel() {}
	~OfobikeWheel() {}
	virtual int GetRadius() const { return OFOBIKE_WHEEL_RADIUS; }
};

//自行车,由车把和车轮组成
class Bike
{
public:
	Bike() :m_pHandlerbar(nullptr),m_pWheel(nullptr)
	{  }
	~Bike() 
	{
		delete m_pHandlerbar; m_pHandlerbar = nullptr;
		delete m_pWheel; m_pWheel = nullptr;
	}
	void AssembleWheel(Wheel* p) { m_pWheel = p; }
	void AssembleHandlerbar(Handlerbar* p) { m_pHandlerbar = p; }

	void DumpInfo()
	{
		if(m_pHandlerbar)
			std::cout << "handlerbar,lenth=" << m_pHandlerbar->GetLength() << std::endl;
		if(m_pWheel)
			std::cout << "wheel,radius=" << m_pWheel->GetRadius() << std::endl;
	}
private:
	Handlerbar* m_pHandlerbar;
	Wheel* m_pWheel;
};
//builder
class BikeBuilder
{
public:
	virtual void ManufactureBike() {}
	virtual void ManufactureWheel() {}
	virtual void ManufactureHandlerbar() {}
	virtual Bike* GetBike() { return nullptr; }
};

//小黄车builder
class OfoBuilder :public BikeBuilder
{
public:
	OfoBuilder():m_pBike(nullptr)
	{}
	~OfoBuilder()
	{
		delete m_pBike;
		m_pBike = nullptr;
	}
	virtual void ManufactureBike() 
	{ 
		m_pBike = new Bike;
	}
	virtual void ManufactureWheel() 
	{
		m_pBike->AssembleWheel(new OfobikeWheel);
	}
	virtual void ManufactureHandlerbar()
	{
		m_pBike->AssembleHandlerbar(new OfobikeHandlerbar);
	}
	virtual Bike* GetBike()
	{ 
		return m_pBike; 
	}
private:
	Bike* m_pBike;
};

//导向器，控制产品的构建过程
class Director
{
public:
	Director(BikeBuilder& p):m_pBuilder(&p)
	{

	}
	Bike* CreateBike()
	{
		if (!m_pBuilder)
			return nullptr;
		//产品的构建过程；用户可控制该过程，可改变产品内部表示；这里将自行车构造和表示的代码分开
		m_pBuilder->ManufactureBike();
		m_pBuilder->ManufactureHandlerbar();
		m_pBuilder->ManufactureWheel();
		return m_pBuilder->GetBike();
	}

private:
	BikeBuilder* m_pBuilder;
};

int main()
{
	OfoBuilder builder;
	std::shared_ptr<Director> pDirector=std::make_shared<Director>(builder);
	auto p = pDirector->CreateBike();
	p->DumpInfo();
	getchar();
    return 0;
}

~~~
