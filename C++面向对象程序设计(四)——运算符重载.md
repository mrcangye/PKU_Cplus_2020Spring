# C++面向对象程序设计(四)——运算符重载

[toc]

本文是中国大学MOOC，北京大学[程序设计与算法（三）C++面向对象程序设计](https://www.icourse163.org/learn/PKU-1002029030#/learn/announce)第四周笔记。本课程学习的[github仓库](https://github.com/mrcangye/PKU_Cplus_2020Spring)欢迎Fork

之前的第三周有部分题目不会，所以测验程序的答案延后发布。

这周主要是面向对象的重载知识点。这些知识在其他面向对象语言也是可以类比贯通的。

## 为什么要使用运算符重载？

在`C++`中，`C++`预定义的运算符，只能用于基本的数据类型的运算。对于其他的，例如复数之间的直接运算是不允许的。

所以我们希望，对象之间也能够通过运算符进行运算。这就是运算符重载。

## 什么是运算符重载？

- 对已有的运算符赋予多重的含义，使同一运算符作用于不同类型的数据，产生不一样的行为。

- 运算符重载实质是函数的重载，可以重载为普通函数，也可以重载为成员函数。

- 把含运算符的表达式转化为对运算符函数的调用。

- 操作数转换成运算符函数的参数。

- 当函数被多次重载时，根据实参的类型决定调用哪个运算符函数。

例如：

```cpp
complex_a + complex_b
5 + 4 = 9
```

## 运算符重载的形式

```cpp
返回值类型 operator运算符 (形参表)
{
    ......
}
```

例如：

```cpp
class Complex
{
    public:
    	double real, imag;
    	Complex(double r = 0.0, double i = 0.0):real(r),imag(i){}
    	Complex operator- (const Complex & c);
};

Complex operator-(const Complex &a, const Complex &b)
{
    return Complex( a.real + b.real , a.imag + b.imag );
}

Complex operator::operator-(const Complex & c)
{
    return Complex(real - c.real, imag - c.imag);
}


int main()
{
    Complex a(4,4),b(1,1),c;
    c = a + b;
    // c = a + b 等价于c = operator+(a,b); 普通函数
    cout << c.real << "," << c.imag << endl;
    cout << (a-b).real << "," << (a-b).imag << endl;
    //a - b 等价于a.operator-(b); 成员函数
    return 0;
}

输出：
5,5
3,3
```

注意：

**重载为成员函数时，参数个数为运算符目数减 1**

**重载为普通函数时，参数个数为运算符目数**

## 赋值运算符 = 的重载

赋值运算符只能重载为成员函数

```cpp
class String{
    private:
    	char * str;
    public:
    	String ():str(new char[1]){str[0] = 0;}
    	const char * c_str(){ return str;};
    	String & operator = {const char * s};
    	String::~String() {delete [] str;}
}

String & String::operator = （const chat * s)
{
    delete [] str;
    str = new char [strlen(s)+1];
    strcpy(str,s);
    return * this;
}

int main(){
    String s;
    s = "Good Luck,";
    //等价于 s.operator=("Good Luck,");
    cout << s.c_str() << endl;
    
    s = "Shenzhou 8!";
   //等价于s.operator=("Shenzhou 8!");
    cout << s.c_str() << endl;
    return 0;
}

输出：
Good Luck,
Shenzhou 8!
```

### 浅拷贝和深拷贝

```cpp
class String{
    private:
    	char * str;
    public:
    	String():str(new char[1]){str[0] = 0;}
    	const char * c_str(){return str;};
    	String & operator = (const char & s){
            if( this == &s )
                return * this;
            delete [] str;
            str = new char [strlen(s.str)+1];
            strcpy(str,s.str);
            return * this;
        };
    	String(String & s)
        {
            str = new char[strlen(s.str)+1];
            strcpy(str,s.str);
        }
    ~String(){delete [] str;}
};
```

## 运算符重载为友元函数

