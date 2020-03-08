# C++面向对象程序设计(一)——从C到C++

本文是中国大学MOOC，北京大学[程序设计与算法（三）C++面向对象程序设计](https://www.icourse163.org/learn/PKU-1002029030#/learn/announce)第一周笔记。

## 一 引用

引用的格式：`类型名 & 引用名 = 某变量名`

```cpp
int & r = n;				// r 引用了 n, r 的类型是 int &
```

某个变量的引用，其实相当于这个变量的另一个名字，二者是等价的

### 案例1.1

```cpp
int n = 4;
int & r = n;	// r 与 n 等价
r = 4;
cout << r;	//输出 4
cout << n;	// r 与 n 等价，因为 r = 4, 所以 n = r = 4
n = 5;
cout << r;	// r 与 n 等价，因为 n = 5, 所以 r = n = 5
```

所以只要等价的其中一个变化了，另一个也要变化



值得注意的是：

- 定义引用时要将其初始化成引用某个变量。就跟初始化变量时尽量赋值是一个道理
- 初始化后，引用的就是这个变量，不会再引用别的变量了，从一而终
- 引用只引用变量，不能引用常量和表达式



### 案例1.2

```cpp
double a = 4, b = 5;
double & r1 = a;
double & r2 = r1;	//r2 , r1 , a 这三者是等价的
r2 = 10;
cout << a <<endl;	//由于 r2 = 10, 所以 a = r1 = r2 =10
r1 = b;
cout << a <<endl;	//由于 r1 = b, 所以 a = r2 = r1 = b = 5
```



### 案例1.3 交换函数

```cpp
// C 语言
void swap( int * a, int * b)
{
    int tmp;
    tmp = * a;
    * a = * b;
    * b = tmp;
}
int n1, n2;
swap(& n1, & n2);
```

```cpp
// C++ 语言
void swap( int & a, int & b)
{
    int tmp;
    tmp =  a;
     a =  b;
     b = tmp;
}
int n1, n2;
swap(n1, n2);
```

对比发现 C++ 的引用更为方便

### 案例1.4 作为函数的返回值

```cpp
int n = 4;
int & SetValue(){	return n;	}
int main()
{
    SetValue() = 40;
    cout << n;
    return 0;
}//输出40
```

## 二 常引用

在引用前加`const`即为常引用

```cpp
int n;
const int & r = n;	//	r 的类型是 const int &
```

值得注意的是：

- 不能通过常引用去修改引用的内容，要不然就不是常引用了

### 案例2.1

```cpp
int n = 100;
const int & r = n;
r = 200;	//编译错，n 可以改，r 不可以改
n = 300;	//没问题
```

### 常引用与引用的转换

1. `const T &` 和 `T &`是不同的类型
2. `T &`类型的引用或T类型的变量可以用来初始化`const T &`类型的引用
3. `const T `类型的常变量和`const T &`类型的不能用来初始化`T &`类型的引用，除非使用强制转换

------

### 例题

#### 1.下面程序片段哪个没错？

A.	int n = 4;	int & r =n * 5;		//引用不可以出现 `n * 5`

B.	int n = 6;	const int & r = n;	r = 7;	//常引用不可以出现`r = 7`,不能通过常引用去修改引用的内容，要不然就不是常引用了

C.	int n = 8;	const int & r1 = n;	int & r2 = r1	//常引用不可以出现`int & r2 = r1`,`const T `类型的常变量和`const T &`类型的不能用来初始化`T &`类型的引用，除非使用强制转换

D.	int n = 8;	int & r1 = n;	const int r2 =r1;

#### 2.下面程序片段输出结果是什么？

```cpp
int a = 1,b = 2;
int & r = a;	//r 与 a 等价
r = b;
r = 7;
cout << a <<endl;	//输出 7
```

## 三 const关键字

`#define`类似于文档的全部替换，不会考虑上下文的意思。在C++中，推荐使用`const`

const int MAX_VAL = 23;

const string SCHOOL_NAME = "Peking University"

值得注意的是：

### 1.不可以通过常量指针修改其指向的内容

```cpp
int n, m;
const int * p = & n;
* p = 5;	//编译出错
n = 4;	//ok
p = & m;	//ok
```

### 2.不能把常量指针赋给非常量指针，反过来是可以的

```cpp
const int * p1;
int * p2;
p1 = p2;	//ok
p2 = p1;	//error
p2 = (int * ) p1;	//ok，强转
```

### 3.函数的参数为常量指针时，我们可以用来避免函数内部不小心改变参数指针所指地方的内容

```cpp
void MyPrintf( const char * p)
{
    strcpy(p,"this");	//编译出错
    printf("%s",p);	//ok
}
```

### 4.不能通过常引用修改其引用的变量

```cpp
int n;
const int & r = n;
r = 5;	//error
n = 4;	//ok
```

------



### 例题

1.下面说法哪种是对的？

A.常引用所引用的变量，其值不能被修改

B.不能通过修改常量指针。去修改其指向的变量	//正确

C.常量指针一旦指向某个变量，就不能再指向其他变量



## 四 new实现动态内存分配

### 1.分配一个变量

```cpp
P = new T;	//T 是任意类型名，P 是类型为 T * 的指针
```

该语句动态分配处一片大小为`sizeof(T)`字节的内存空间，并将该空间的**起始地址**赋值给P

#### 案例4.1

```cpp
int * pn;
pn = new int;
* pn = 5;
```

### 2.分配一个数组

```cpp
P = new T[N];	//T 为任意类型名；P 为类型为 T * 的指针；N 要分配到数组元素的个数，可以是整型表达式
```

该语句动态分配处一片大小为`sizeof(T) * N`字节的内存空间，并将该空间的**起始地址**赋值给P

#### 案例4.2

```cpp
int * pn;
int i = 5;
pn = new int[i * 20];
pn[0] = 20;
pn[100] = 30;	//数组越界
```

## 五.用delete释放动态分配的内存

用了`new`动态分配一定要用`delete`释放

### 案例5.1

```cpp
int * p = new int;
* p = 5;
delete p;
delete p;	//异常，一个new只能delete一次
```

### 案例5.2

```cpp
//delete动态分配的数组
int * p = new int[20];
p[0] = 1;
delete [] p;
```

------

### 例题

#### 1.表达式 new int 的返回值类型是？

A int 

B int *	//正确

C int &

#### 2.下面哪个正确？

A char * p = new int;	p = ' a ' ;	delete p ;

B int * p =new int [25];	p[10] = 100;	delete p;

C char * p = new char[10];	p[0] = 'K';	delete [] p;	//正确

## 六 内联函数

```cpp
inline int Max(int a, int b)
{
    if( a > b)	return a;
    return b;
}
```

## 七 函数重载

一个或多个函数，名字相同，然而参数个数或参数类型不相同

例如：

```cpp
int Max(double f1, double f2){}	//Max(3.4, 2.5)
int Max(int n1, int n2){}	//Max(2,4)
int Max(int n1, int n2,int n3){}	//Max(1,2,3)
```

## 八 缺省函数

定义函数时，最右边的连续若干个参数有缺省值，那么在调用时候，如果那些位置不写参数，参数就是缺省值

```cpp
void func(int x1, int x2 = 2, int x3 = 3){}
func(10);	//等效于func(10,2,3)
func(10, 8);	//等效于func(10,8,3)
func(10, , 8);	//错误，只能最右边连续若干个缺省
```



