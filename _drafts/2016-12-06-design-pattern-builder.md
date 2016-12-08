---
layout: page
title: "设计模式-抽象工厂模式"
comments: true
tags: [Design Pattern]
categories: Design Pattern
header:
   image_fullwidth: "header_unsplash_library.jpg"
---

# 意图

**提供**一个**创建**一系列**相关或相互依赖对象**的**接口**，而无需指定他们具体的类

# 类图

<img src="http://yuml.me/diagram/plain/class/
[Client]->[Abstract Facctory],[Client]->[AbstractProductA],[Client]->[AbstractProductB],
[Abstract Facctory|+CreateProduct1;+CreateProduct2]^-[Product1Factory|+CreateProduct1;+CreateProduct2],[Abstract Facctory]^-[Product2Factory|+CreateProduct1;+CreateProduct2],
[AbstractProductA]^-[ConcreteProductA1],[AbstractProductA]^-[ConcreteProductA2],[AbstractProductB]^-[ConcreteProductB1],[AbstractProductB]^-[ConcreteProductB2],
[Product1Factory]-.->[ConcreteProductA1],[Product1Factory]-.->[ConcreteProductB1],
[Product2Factory]-.->[ConcreteProductA2],[Product2Factory]-.->[ConcreteProductB2]
">


# 适用性

* 一个系统要独立于它的产品的创建、组合和表示时
* **一个系统要由多个产品系列中的一个来表示时**
* 当你要强调一系列相关的产品对象的设计以便进行联合使用时
* 当你提供一个产品类库,而只想显示它们的接口而不是实现时

# 参与者

* Client:仅使用由AbstractFactory和AbstractProduct类声明的接口
* AbstractFactory:声明一个创建抽象产品对象的操作接口,AbstractFactory将产品对象的创建延迟到它的子类
* ProductFactory:实现创建具体产品对象的操作
* AbstractProduct:为一类产品对象声明一个接口
* ContreteProduct:定义一个将被相应的具体工厂创建的产品对象


# 优缺点

* **它分离了具体的类**:客户通过它们的抽象接口操纵实例。产品的类名也在具体工厂的实现中被分离；它们不出现在客户代码中
* **它使得易于交换产品系列**:一个具体工厂类在一个应用中仅出现一次—即在它初始化的时候。这使得改变一个应用的具体工厂变得很容易。它只需改变具体的工厂即可使用不同的产品配置，这是因为一个抽象工厂创建了一个完整的产品系列，所以整个产品系列会立刻改变
* **它有利于产品的一致性**:当一个系列中的产品对象被设计成一起工作时，一个应用一次只能使用同一个系列中的对象
* **难以支持新种类的产品**:难以扩展抽象工厂以生产新种类的产品。这是因为AbstractFactory接口确定了可以被创建的产品集合。 支持新种类的产品就需要扩展该工厂接口，这将涉及 AbstractFactory类及其所有子类的改变

# 举例

![mobike-vs-ofobike](/images/2016/20161129-mobikevsofobike.jpg)

这里我们假设自行车由车轮和车把组成，而忽略车座，铃铛和脚蹬等部分。MobikeFactory成产摩拜单车Mobike，OfoFactory生产ofo小黄车ofobike。

~~~ cpp
// 01abstractfactory.cpp : Defines the entry point for the console application.
//演示抽象工厂模式

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
//抽象工厂
class BikeFactory
{
public:
	BikeFactory() {}
	virtual ~BikeFactory()
	{
		for (auto& ptr : handlerbarCon)
		{
			delete ptr;
		}
		handlerbarCon.clear();
		for (auto& ptr : wheelCon)
		{
			delete ptr;
		}
		wheelCon.clear();
	}
	virtual Handlerbar* ManufactureHandlerbar() { return nullptr; }
	virtual Wheel* ManufactureWheel() { return nullptr; }

protected:
	std::vector<Handlerbar*> handlerbarCon;
	std::vector<Wheel*> wheelCon;
};
//工厂1：生产摩拜单车
class MobikeFactory :public BikeFactory
{
public:
	virtual Handlerbar* ManufactureHandlerbar()
	{
		auto hb= new MobikeHandlerbar;
		handlerbarCon.push_back(hb);
		return hb;
	}
	virtual Wheel* ManufactureWheel()
	{
		auto mw= new MobikeWheel;
		wheelCon.push_back(mw);
		return mw;
	}
};
//工厂1：生产ofo小黄车
class OfobikeFactory :public BikeFactory
{
public:
	virtual Handlerbar* ManufactureHandlerbar()
	{
		auto hb = new OfobikeHandlerbar;
		handlerbarCon.push_back(hb);
		return hb;
	}
	virtual Wheel* ManufactureWheel()
	{
		auto ow = new OfobikeWheel;
		wheelCon.push_back(ow);
		return ow;
	}
};

class Client
{
public:
	void MakeMobike()
	{
		BikeFactory* bf = new MobikeFactory;
		Handlerbar* hb = bf->ManufactureHandlerbar();
		Wheel* wl = bf->ManufactureWheel();

		std::cout << "Mobike Handlerbar length=" << hb->GetLength() << std::endl;
		std::cout << "Mobike Wheel radius=" << wl->GetRadius() << std::endl;
	}
	void MakeOfobike()
	{
		BikeFactory* bf = new OfobikeFactory;
		Handlerbar* hb = bf->ManufactureHandlerbar();
		Wheel* wl = bf->ManufactureWheel();

		std::cout << "Ofobike Handlerbar length=" << hb->GetLength() << std::endl;
		std::cout << "Ofobike Wheel radius=" << wl->GetRadius() << std::endl;
	}
};

int main()
{
	std::shared_ptr<Client> pclient(new Client);
	pclient->MakeMobike();
	pclient->MakeOfobike();
	system("pause");
    return 0;
}

~~~
