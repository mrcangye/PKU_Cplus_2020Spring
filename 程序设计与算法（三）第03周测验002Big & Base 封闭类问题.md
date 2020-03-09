# 程序设计与算法（三）第03周测验002:Big & Base 封闭类问题

本文是中国大学MOOC，北京大学[程序设计与算法（三）C++面向对象程序设计](https://www.icourse163.org/learn/PKU-1002029030#/learn/announce)第三周测验。本课程学习的[github仓库](https://github.com/mrcangye/PKU_Cplus_2020Spring)欢迎Fork

- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  程序填空，输出指定结果

  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;
  class Base {
  public:
  	int k;
  	Base(int n):k(n) { }
  };
  class Big
  {
  public:
  	int v;
  	Base b;
  // 在此处补充你的代码
  };
  int main()
  {
  	int n;
  	while(cin >>n) {
  		Big a1(n);
  		Big a2 = a1;
  		cout << a1.v << "," << a1.b.k << endl;
  		cout << a2.v << "," << a2.b.k << endl;
  	}
  }
  ```

  

- 输入

  多组数据，每组一行，是一个整数

- 输出

  对每组数据，输出两行，每行把输入的整数打印两遍

- 样例输入

  ```cpp
  3
  4
  ```

  

- 样例输出

  ```cpp
  3,3
  3,3
  4,4
  4,4
  ```

  老规矩，先看主函数：

  ```cpp
  int main()
  {
  	int n;
  	while(cin >>n) {
  		Big a1(n);		//构造函数,初始化a1
  		Big a2 = a1;	//复制构造函数，复制a1
  		cout << a1.v << "," << a1.b.k << endl;
  		cout << a2.v << "," << a2.b.k << endl;
  	}
  }
  ```

  再看类函数：

  ```cpp
  class Base {
  public:
  	int k;
  	Base(int n):k(n) { }
  };
  
  class Big	//有成员变量的类叫封闭类
  {
  public:
  	int v;
  	Base b;	//成员对象
  // 在此处补充你的代码
  };
  ```

  注意，这是一个封闭类，课程的PPT有讲到：

  **任何生成封闭类对象的语句，都要使得编译器明白，对象中的成员对象，是如何初始化的。完成这一任务的方法是通过封闭类的构造函数的初始化列表，成员对象初始化列表中的参数可以是任意复杂的表达式，可以包括函数，变量，只要表达式中的函数或变量有定义就行。**

  **如果题目不懂，不妨把课程对应部分再看一遍。慢慢来，比较快。不要心浮气躁。**

  所以我们需要使用初始化列表进行封闭类的初始化：

  ```cpp
  class Big
  {
  public:
      int v;
      Base b;
  // 在此处补充你的代码
      Big(int n):v(n),b(n){}	//初始化列表
  };
  ```

  提交，通过。记得下载通过码。

  

