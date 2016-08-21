---
layout: page
title: "boost-bind-[translate]"
comments: true
tags: [boost C++ bind]
categories: boost
header:
   image_fullwidth: "header_unsplash_library.jpg"
---
#Boost.Bind

[原文地址](http://www.boost.org/doc/libs/1_59_0/libs/bind/doc/html/bind.html#bind.purpose.using_bind_with_functions_and_fu)

# 目的

boost::bind是 std::bind1st and std::bind2nd的泛化版本. 它支持函数对象(function objects),仿函数( functions),函数指针(function pointers),以及成员函数指针(member function pointers), 并可绑定任意个参数到任意位置. bind不需要使用typedef定义result_type、first_type、second_type，所以它对函数对象没有特殊要求。

# 绑定functions和function pointers到bind
假如有以下定义:

~~~cpp

int f(int a, int b)
{
    return a + b;
}

int g(int a, int b, int c)
{
    return a + b + c;
}

~~~ 

bind(f, 1, 2) 会生成一个不带参数的函数对象，并返回 f(1, 2). 同样的， bind(g, 1, 2, 3)() 相当于 g(1, 2, 3).

我们可以有选择的将参数绑定到bind上.如： bind(f, \_1, 5)(x)相当于 f(x, 5); 这里的\_1 是一个占位符，其所代表的意义是在函数调用时使用第一个参数来替换\_1

作为比较，这里举几个使用标准bind函数的例子:

~~~cpp

std::bind2nd(std::ptr_fun(f), 5)(x);
std::bind1st(std::ptr_fun(f), 5)(x);   // f(5, x)
bind(f, 5, _1)(x);                     // f(5, x)

~~~

bind 可以绑定任意多个参数，并且它的函数体会规则非常通用:

~~~cpp

bind(f, _2, _1)(x, y);                 // f(y, x)
bind(g, _1, 9, _1)(x);                 // g(x, 9, x)
bind(g, _3, _3, _3)(x, y, z);          // g(z, z, z)
bind(g, _1, _1, _1)(x, y, z);          // g(x, x, x)

~~~

我们注意到在最后一个例子中bind(g, _1, _1, _1) 只使用了第一个参数，其他参数被悄悄的忽略了( silently ignored), 第三个例子中类似，只使用了第三个参数z，前两个x、y被silently ignored.

**The arguments that bind takes are copied and held internally by the returned function object.**(注意这里，bing参数是被copy了一份，其生成的函数对象所使用的并不是参数本身，而是其copy；之前因为忽略了这一点，导致工作中在使用shard_ptr时引用计数不正确)

~~~cpp

int i = 5;
bind(f, i, _1);

~~~

bind所生成的 function object会持有一份i的拷贝而不是i的引用. 如果我们不想使用拷贝而就是想使用参数的引用，可以通过boost::ref and boost::cref 来实现:

~~~cpp
int i = 5;
bind(f, ref(i), _1);
bind(f, cref(i), _1);

~~~

# 绑定函数对象到bind

bind 不仅可以绑定函数，还可以绑定函数对象:

~~~cpp

struct F
{
    int operator()(int a, int b) { return a - b; }
    bool operator()(long a, long b) { return a == b; }
};

F f;
int x = 104;
bind<int>(f, _1, _1)(x);		// f(x, x), i.e. zero

~~~

一些编译器并不支持bind<R>(f, ...) 这种语法. 我们可以使用如下方式:

~~~cpp

boost::bind(boost::type<int>(), f, _1, _1)(x);

~~~

这里需要注意，该写法不是标准使用方式，只是一种可使用的替换方式.

如果函数对象内容使用typedef定义了 result_type, 那么bind的必须显示指定返回值类型:

~~~cpp

int x = 8;
bind(std::less<int>(), _1, 9)(x);	// x < 9

~~~

[注意: 省略返回值类型的写法并非所有编译器都支持.]

默认情况下bind会持有一份函数对象的拷贝，当一个函数对象是non-copyable时、拷贝的代价太大时或者内部有自身状态时可以使用 boost::ref 和 boost::cref .

~~~cpp

struct F2
{
    int s;

    typedef void result_type;
    void operator()(int x) { s += x; }
};

F2 f2 = { 0 };
int a[] = { 1, 2, 3 };
std::for_each(a, a+3, bind(ref(f2), _1));
assert(f2.s == 6);

~~~

# 绑定bind到成员函数

指向数据成员的指针和指向成员函数的指针都不是函数对象, 因为它们不支持 operator(). 为了方便，bind的第一个参数可以是成员函数指针, 其行为类似于boost::mem_fn 所做的事情，即将成员函数指针转换为函数对象.

~~~cpp

bind(&X::f, args)
~~~
相当于

~~~cpp

bind<R>(mem_fn(&X::f), args)
~~~
这里的 R 是X::f (for member functions)的返回类型 或者类成员的类型 (for data members.)

[注意: mem\_fn 会生成一个函数对象,这个函数对象的第一个参数可以是 pointer, reference, smart pointer; 详细用法可参考 [mem_fn](http://www.boost.org/doc/libs/1_59_0/libs/bind/doc/html/mem_fn.html) 文档.]

~~~cpp

struct X
{
    bool f(int a);
};

X x;
shared_ptr<X> p(new X);
int i = 5;

bind(&X::f, ref(x), _1)(i);		// x.f(i)
bind(&X::f, &x, _1)(i);			// (&x)->f(i)
bind(&X::f, x, _1)(i);			// (internal copy of x).f(i)
bind(&X::f, p, _1)(i);			// (internal copy of p)->f(i) **会增加引用计数**
~~~

最后两个例子生成了"self-contained" 函数对象。 bind(&X::f, x, \_1) 持有一份x的拷贝， bind(&X::f, p,\ _1)持有一份p的拷贝；因为p是boost::shared_ptr,所以函数对象持有一份x实例的引用(这里x的引用计数会增加)；即便p超过其作用域、或者reset()，x也不会被释放。

# Using nested binds for function composition
有时候bind的参数可能是bind表达式自身：

~~~cpp

bind(f, bind(g, _1))(x);               // f(g(x))
~~~

在外层bind被评估(evaluated)之前，内层bind会首先被评估(evaluated). 在上面的例子中，函数对象在调用x之前，会首先计算 bind(g, _1)(x) ，这个时候会执行g(x),然后将 bind(f, g(x))(x) 的结果给了f,最终相当于f(g(x)).

bind可以用来执行函数比较，在bind_as_compose.cpp 有个例子演示了如何使用bind来实现Boost.Compose相似的功能。

*以下一段未曾亲身尝试，保留原文*

>Note that the first argument - the bound function object - is not evaluated, even when it's a function object that is produced by bind or a placeholder argument, so the example below does not work as expected:

> typedef void (*pf)(int);

> std::vector<pf> v;
std::for_each(v.begin(), v.end(), bind(_1, 5));
The desired effect can be achieved via a helper function object apply that applies its first argument, as a function object, to the rest of its argument list. For convenience, an implementation of apply is provided in the apply.hpp header file. Here is how the modified version of the previous example looks like:

> typedef void (*pf)(int);

> std::vector<pf> v;
std::for_each(v.begin(), v.end(), bind(apply<void>(), _1, 5));
Although the first argument is, by default, not evaluated, all other arguments are. Sometimes it is necessary not to evaluate arguments subsequent to the first, even when they are nested bind subexpressions. This can be achieved with the help of another function object, protect, that masks the type so that bind does not recognize and evaluate it. When called, protect simply forwards the argument list to the other function object unmodified.

> The header protect.hpp contains an implementation of protect. To protect a bind function object from evaluation, use protect(bind(f, ...)).

> Overloaded operators (new in Boost 1.33)
For convenience, the function objects produced by bind overload the logical not operator ! and the relational and logical operators ==, !=, <, <=, >, >=, &&, ||.

> !bind(f, ...) is equivalent to bind(logical_not(), bind(f, ...)), where logical_not is a function object that takes one argument x and returns !x.

> bind(f, ...) op x, where op is a relational or logical operator, is equivalent to bind(relation(), bind(f, ...), x), where relation is a function object that takes two arguments a and b and returns a op b.

> What this means in practice is that you can conveniently negate the result of bind:

> std::remove_if(first, last, !bind(&X::visible, _1)); // remove invisible objects
and compare the result of bind against a value:

> std::find_if(first, last, bind(&X::name, _1) == "Peter");
std::find_if(first, last, bind(&X::name, _1) == "Peter" || bind(&X::name, _1) == "Paul");
against a placeholder:

> bind(&X::name, _1) == _2
or against another bind expression:

> std::sort(first, last, bind(&X::name, _1) < bind(&X::name, _2)); // sort by name


# Using bind with standard algorithms
~~~cpp
class image;

class animation
{
public:
    void advance(int ms);
    bool inactive() const;
    void render(image & target) const;
};

std::vector<animation> anims;

template<class C, class P> void erase_if(C & c, P pred)
{
    c.erase(std::remove_if(c.begin(), c.end(), pred), c.end());
}

void update(int ms)
{
    std::for_each(anims.begin(), anims.end(), boost::bind(&animation::advance, _1, ms));
    erase_if(anims, boost::mem_fn(&animation::inactive));
}

void render(image & target)
{
    std::for_each(anims.begin(), anims.end(), boost::bind(&animation::render, _1, boost::ref(target)));
}
~~~
# 绑定Boost.Function到bind

~~~cpp

class button
{
public:
    boost::function<void()> onClick;
};

class player
{
public:
    void play();
    void stop();
};

button playButton, stopButton;
player thePlayer;

void connect()
{
    playButton.onClick = boost::bind(&player::play, &thePlayer);
    stopButton.onClick = boost::bind(&player::stop, &thePlayer);
}

~~~

# 小结
+ 之所以有本文是因为在工作中遇到了问题，忽略了bind会增加引用计数；这次决定看一下bind用法
+ 翻译真心不易，看明白不难，写明白不易
+ 知易行难，知行合一，且行且珍惜