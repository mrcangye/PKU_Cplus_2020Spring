# C++面向对象程序设计(二)——类和对象基础

本文是中国大学MOOC，北京大学[程序设计与算法（三）C++面向对象程序设计](https://www.icourse163.org/learn/PKU-1002029030#/learn/announce)第二周笔记。

面向对象的程序 = 类 + 类 + ... 类，设计程序的过程就是设计类的过程

面向对象的程序设计有：**抽象**，**封装**，**继承**，**多态**四个基本特点

## 一 使用类的成员变量和成员函数的几种方法

### 1.对象名.成员名

```cpp
CRectangle r1, r2
r1.w = 5;
r2.Init(5,4);
```

### 2.指针—>成员名

```cpp
CRectangle r1, r2;
CRectangle * p1 = & r1;
CRectangle * p2 = & r2;
p1 —> w = 5;
p2 —> Init(5,4);
```

### 3.引用名.成员名

```cpp
CRectangle r2;
CRectangle & rr = r2;
rr.w = 5;
rr.Init(5,4);
```

## 二 类成员可访问的范围

### private

私有成员，只能在成员函数内访问

### public

公有成员，可以在任何地方访问

### protected

保护成员

### 案例2.1

```cpp
class className{
    private：
        私有属性和函数
    public：
        公有属性和函数
    protected:
    	保护属性和函数
}；
```



### 案例2.2 如果某个成员前面没有上述关键字，则缺省被认为是私有成员

```cpp
class Man{
    int nAge;	//私有成员
    char szName[20];	//私有成员
public:
    void SetName(char * szName){
        strcpy(	Man::szName, zsName );
    }
};
```

在类成员函数内部，能够访问：

1. 当前对象的全部属性，函数
2. 同类其他对象的全部属性，函数

在类成员函数以外的地方，只能访问该类对象的公有成员

### 案例2.3

```cpp
class CEmployee{
    private:
    	char szName[30];	//名字
    public:
    	int salary;	//工资
    	void setName(char * name);
    	void getName(char * name);
    	void averageSalary(CEmployee e1,CEmployee e2);
};

void CEmployee::setName(char * name){
    strcpy(szName, name);	//ok
}

void CEmployee::getName(char * name){
    strcpy(name, szName);	//ok
}

void CEmployee::averageSalary(CEmployee e1, CEmplyee e2){
    cout << e1.szName;	//ok
    salary = (e1.salary + e2.salary) / 2;
}

int main(){
    CEmployee e;
    strcpy(e.szName, "Tom1234567889");	//error，编译错，不能访问私有成员
    e.setName( "Tom" );	//ok
    e.salary = 5000;	//ok
    return 0;
}
```



设置私有成员的机制，叫**隐藏**

目的是强制对成员变量的访问通过成员函数进行，如果以后成员变量的类型等属性修改后，只需要更改成员函数即可。

## 三 struct定义的类

```cpp
struct CEmployee{
    	char szName[30];	//公有
    public:
    	int salary;	//工资
    	void setName(char * name);
    	void getName(char * name);
    	void averageSalary(CEmployee e1,CEmployee e2);
};
```

`struct`定义的类与`class`的区别是，未说明公有还是私有的成员，按公有成员看待

## 四 成员函数的重载和缺省

成员函数也是可以重载和缺省的

```cpp
#include <iostream>
using namespace std;
class Location{
    private:
    	int x, y;
    public:
    	void init( int x = 0, int y = 0 );
    	void valueX( int val = 0 ){ x = val; }	//重载
    	int valueX(){ return x; }
};
```

当然，在使用的时候要避免二义性：

```cpp
//对于上面的类
Location A;
A.valueX();	//出错，编译器不知道改使用哪个valueX
```

## 五 构造函数

### 基本概念

- 构造函数是成员函数的一种，可以有参数，但是**不能有返回值(void也不行)**
- 作用是对对象初始化，如给成员变量赋值，有了构造函数就不需要专门再写初始化函数，也不用担心忘记调用初始化函数。
- 有的对象没被初始化就使用，会导致程序出错
- 如果定义类时没写构造函数，则编译器生成一个默认的无参数的构造函数，默认的构造函数无参数，不做任何操作
- 当然，如果定义了构造函数，那么编译器不会生成默认的构造函数
- 对象生成时构造函数会自动被调用，对象一旦生成，就再也不能在其上执行构造函数
- 一个类可以有多个构造函数
- 构造函数的函数名一般是类的名字，且不需要返回值，所以没有int X()之类的定义

```cpp
class Complex{
    private：
        double real,imag;
    public:
    	void Set( double r, double i);
};	//生成默认构造函数
Complex c1;		//默认构造函数被调用
Complex * pc = new Complex;		//默认构造函数被调用
```

```cpp
class Complex{
    private：
        double real,imag;
    public:
    	void Set( double r, double i);
};
Complex::Complex( double r, double i){
    real = r, image = i;
}
Complex c1;		//缺少构造函数的参数
Complex * pc = new Complex;		//error 没有参数
Complex c1(2);	//ok
```

构造函数毕竟时成员函数的一种，所以它可以有多个构造函数，参数个数或者类型可以不同

```cpp
class Complex{
    private：
        double real,imag;
    public:
    	void Set( double r, double i);
    Complex(double r, double i);
    Complex(double r);
    Complex(Complex c1, Complex c2);
};

Complex::Complex(double r,double i)
{
    real = r, image = i;
}
Complex::Complex(double r)
{
    real = r, image = 0;
}
Complex::Complex(Complex c1, Complex c2)
{
    real = c1.real + c2.real;
    image = c1.image + c2.image;
}

Complex c1(3), c2(1,0), c3(c1, c2);
```

需要注意的是：

构造函数最好是`public`的，`private`构造函数不能用来初始化对象



### 例题：下面哪条语句编译不会出错？

```cpp
class A{
    int v;
    public:
    	A(int n){ v = n; }
};
```

A. A a1(3);	//正确

B. A a2;

C A * p = new A();

### 构造函数在数组中的使用

```cpp
class CSample{
    int x;
public:
    CSample(){
        cout << "Construtor 1 Called" << endl;
    }
    CSample( int n ){
        x = n;
        cout << "Construtor 2 Called" << endl;
    } 
}

int main(){
    CSample array1[2];
    cout << "step1" << endl;
    CSample array2[2] = {4， 5};
    cout << "step2" << endl;
    CSample array3[2] = {3};
    cout << "step3" << endl;
    CSample * array4 = new CSample[2];
    delete [] array4;
    return 0;
}
```

输出：

```cpp
Construtor 1 Called
Construtor 1 Called
step1
Construtor 2 Called
Construtor 2 Called
step2
Construtor 2 Called
Construtor 1 Called
step3
Construtor 1 Called
Construtor 1 Called
```

### 例题，假设 A 是一个类的名字，下面的语句生成了几个类 A 的对象？

```cpp
A * arr[4] = { new Ａ(), NULL, new A () };
```

A. 1

B. 4

C. 2		//正确

## 六 复制构造函数

### 基本概念

只有一个参数，即对同类对象的引用

形如`X :: X( X & )`或`X :: X( const X & )`

同构造函数一样，如果没有定义复制构造函数，那么编译器生成默认复制构造函数，默认的复制构造函数完成复制功能

```cpp
class Complex{
    private:
    				double real, imag;
};
Complex c1;		//调用缺省无参构造函数
Complex c2( c1 );		//调用缺省无参构造函数，将 c2 初始化成和 c1 一样
```

```cpp
class Complex{
    public:
    				double real, imag;
    Complex( ) 	{	}
    Complex( const Complex & c){
        real = c.real;
        imag = c.imag;
        cout << "Copy Constructor called";
    }
};
Complex c1;
Complex c2( c1 );	//调用自己定义的复制构造函数，输出Copy Constructor called
```

不允许出现` X :: X ( Ｘ )`的构造函数

```cpp
class CSample{
    Csample( CSample c ){	//不允许出现这样的构造函数
        
    }
};
```

### 复制构造函数作用的三种情况

#### 一个对象去初始化同类的另一个对象时

```cpp
Complex c2( c1 );
Complex c2 = c1;	//初始化，并非赋值
```

#### 如果某函数有一个参数是类 A 的对象，那么该函数被调用时，类A的复制构造函数将被调用

```cpp
class A
{
    public:
    	A(){ };
    	A( A & a ){
            cout << " Copy constructor called " << endl;
        }
};
void Func(A a1){}
int main(){
    A a2;
    Func(a2);
    return 0;
}//输出： Copy constructor called
```

如果函数的返回值是类Ａ的对象时，则函数返回时，Ａ的复制构造函数被调用

```cpp
class A
{
    public:
    int v;
    A( int n ){ v = n; };
    A( const A & a){
        v = a.v;
        cout << " Copy construtor called  " << endl;
    }
}
A Func(){
    A b( 4 );
    return b;
}
int main(){
    cout << Func().v <<endl;
    return 0;
}
//输出结果: 
//Copy constructor called
//4
```

但是，对象间的赋值并不会导致复制构造函数被调用

## 七 类型转换构造函数

### 基本概念

目的是实现类型的自动转换

只有一个参数，不是复制构造函数的构造函数，一般就可以看做是转换构造函数

当需要时候，编译系统会自动调用转换构造函数，建立一个无名的临时对象或临时变量

```cpp
class Complex{
    public:
    			double real, imag;
    			Complex( int i ){
                    cout << "IntConstructor called" << endl;
                    real = i;
                    imag = 0;
                }
    			Complex( double r, double i ){
                    real = r;
                    imag = i;
                }
};

int main(){
    Complex c1(7, 8);
    Complex c2 = 12;
    c1 = 9;	//９自动转换为一个临时的Complex对象
    cout << c1.real << "," << c1.imag << endl;
    return 0;
}
```

## 八 析构函数

### 基本概念

名字与类名相同，在前面加上`~`，没有参数和返回值，一个类最多只能有一个析构函数

析构函数对象消亡时即自动被调用。所以析构函数是用来在对象消亡前做善后工作，比如释放分配的空间等

同样的，如果没有定义析构函数，那么编译器会自动生成缺省析构函数，缺省析构函数什么也不做。

```cpp
class String{
    private:
    	char * p;
    public:
    	String(){
            p = new char [10];
        }
    ~ String();
};
String :: ~ String()
{
    delete [] p;
}
```

对于数组，对象数组生命周期结束时，对象数组的每个元素的析构函数都会被调用

```cpp
class Ctest{
    public:
    ~ Ctest()	{	cout << "destructor called" << endl;}
};

int main(){
    Ctest array[2];
    cout << "End Main" << endl;
    return 0;
}
//输出
//End Main
//destructor called
//destructor called
```

### delete 运算导致的析构函数调用

```cpp
Ctest * pTest;
pTest = new Ctest;	//构造函数调用
delete pTest;	//析构函数调用

pTest = new Ctest[3];	//构造调用3次
delete [] pTest;	//析构调用3次
```

### 析构函数在对象作为函数返回值返回后被调用

```cpp
class CMyclass{
    public:
    ~ CMyclass () {
        cout << "destructor" << endl;
    }
};

CMyclass obj;
CMyclass fun( CMyclass sobj ){	//参数对象消亡也会导致析构函数被调用
    return obj;		//函数调用返回时生成临时对象返回
}

int main(){
    obj = fun(obj); 	//函数调用的返回值被用过后，该临时对象析构函数被调用
    return 0;
}
```

