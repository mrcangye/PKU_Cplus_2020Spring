# 程序设计与算法（三）第06周测验003:这是什么鬼delete

本文是中国大学MOOC，北京大学[程序设计与算法（三）C++面向对象程序设计](https://www.icourse163.org/learn/PKU-1002029030#/learn/announce)第六周测验。本课程学习的[github仓库](https://github.com/mrcangye/PKU_Cplus_2020Spring)欢迎Fork

总时间限制: 

1000ms

内存限制: 

65536kB

描述

程序填空输出指定结果

```cpp
#include <iostream> 
using namespace std;
class A 
{ 
public:
	A() { }
// 在此处补充你的代码
}; 
class B:public A { 
	public: 
	~B() { cout << "destructor B" << endl; } 
}; 
int main() 
{ 
	A * pa; 
	pa = new B; 
	delete pa; 
	return 0;
}
```

输入

无

输出

```cpp
destructor B
destructor A
```

先看主函数

```cpp
int main() 
{ 
	A * pa; 
	pa = new B; 
	delete pa; 
	return 0;
}
```

主函数新建了一个`B`类对象，而`B`类是`A`的派生。后面又删除了`B`类对象

再看看类

```cpp
class A 
{ 
public:
	A() { }
// 在此处补充你的代码
}; 
class B:public A { 
	public: 
	~B() { cout << "destructor B" << endl; } 
}; 
```

`B`类里有一个析构函数。

继续看输出

```
destructor B
destructor A
```

明显是析构函数被调用了。

所以我们应该补上`A`的析构函数。

但是，我们通过课程了解到

> 通过基类的指针删除派生类对象时，通常情况下只调用基类的析构函数。但是删除一个派生类对象时，应该先调用派生类的析构函数，然后调用基类的析构函数。这样会冲突
>
> 所以我们要把基类的析构函数声明为virtual

所以这个析构函数应该是虚析构函数

我们就可以这样写

```cpp
virtual ~A(){cout << "destructor A" << endl;} 
```

提交，通过