# c++ 重载

@(c++基础)[重载]

重载是c++提供的一个新特性。c++重载分为函数重载和运算符重载，这两种重载的实质是一样的，因为进行运算可以理解为是调用一个函数。  
通过使用重载机制，可以对一个函数名（或运算符）定义多个函数（或运算功能），只不过要求这些函数的参数（或参加运算的操作数）的类型有所不同。重载使c++程序具有更好的可扩充性。

## 函数重载  
函数重载是指在同一作用域内，可以有一组具有相同函数名，不同参数列表的函数，这组函数被称为重载函数。重载函数通常用来命名一组功能相似的函数，这样做减少了函数名的数量，避免了名字空间的污染，对于程序的可读性有很大的好处。
> When two or more different declarations are specified for a single name in the same scope,  that name is said to overloaded.  By extension, two declarations in the same scope that declare the same name but with different types are called overloaded declarations. Only function declarations can be overloaded; object and type declarations cannot be overloaded. ——摘自《ANSI C++ Standard. P290》

看下面的一个例子：实现一个打印函数，既可以打印int型、也可以打印字符串型。在c++中，我们可以这样做：
```cpp
#include<iostream>
using namespace std;

void print(int i)
{
	cout<<"print a integer :"<<i<<endl;
}

void print(string str)
{
	cout<<"print a string :"<<str<<endl;
}

int main()
{
	print(12);
	print("hello world!");
	return 0;
}
```

1. 不能利用函数返回类型的不同进行函数重载。因为在没有确定调用的是哪个函数之前，不知道函数的返回类型。  
2. 不能利用引用进行函数重载。因为编译器无法决定调用哪一个函数

从上面可以看出，一般函数的重载使C++程序具有更好的可扩充性。此外，类的成员函数也可以重载，特别是构造函数的重载给C++程序设计带来很大的灵活性。

## 运算符重载    
c++中预定义的运算符的操作对象只能是基本数据类型。但实际上，对于许多用户自定义类型（例如类），也需要类似的运算操作。这时就必须在c++中重新定义这些运算符，赋予已有运算符新的功能，使它能够用于特定类型执行特定的操作。运算符重载的实质是函数重载，它提供了c++的可扩展性，也是C++最吸引人的特性之一。    
运算符函数的定义与其他函数的定义类似，惟一的区别是运算符函数的函数名是由关键字`operator`和其后要重载的运算符符号构成的。运算符函数定义的一般格式如下：
```cpp
<返回类型说明符> operator <运算符符号>(<参数表>)
{
	//函数体
}
```
例  定义复数类型，重载运算符`+`。例如：c3=c1+c2
```cpp
class   Complex
{
public:    // 公有成员，以便运算符函数(非成员函数)访问
      float   r;  // 实部
      float   i; // 虚部
public:
      Complex(float  x=0, float  y=0) { r=x; i=y; }
};

Complex   operator+(Complex  c1 , Complex  c2)
{
      Complex  temp;
      temp.r=c1.r+c2.r;
      temp.i=c1.i+c2.i;
      return   temp;
}

void  main()

{
      Complex  complex1(3.34f, 4.8f), complex2(12.8f, 5.2f);

      Complex  complex;

      complex=complex1+complex2;
  // 进行两个复数的相加运算
      cout<<complex.r<<'+'<<complex.i<<'i'<<endl;
}
```
说明：  
- 本例采用普通函数的形式重载运算符。  
- **可以采用成员函数的形式重载运算符。并且如果运算符函数要求直接访问类的非公有成员时，运算符函数不能定义为非成员函数，除非将它声明为该类的友元函数**
- 当利用非成员函数重载双目运算符时，运算符函数的第一个参数代表运算符左边的操作数，运算符函数第二个参数代表运算符右边的操作数。  
- **当利用成员函数重载双目运算符时，运算符左边的操作数就是对象本身，不能再将它作为运算符函数的参数，运算符函数只需要一个函数参数。**


```cpp
class Point    
{    
private:    
    int x;   
public:    
    Point(int x1)  
    {   
        x=x1;
    }    
    Point(Point& p)     
    {   
        x=p.x;
    }  
    const Point operator+(const Point& p);//使用成员函数重载加号运算符  
    friend const Point operator-(const Point& p1, const Point& p2);//使用友元函数重载减号运算符  
};    
  
const Point Point::operator+(const Point& p)  
{  
    return Point(x+p.x);  
}  
  
Point const operator-(const Point& p1, const Point& p2)  
{  
    return Point(p1.x-p2.x);  
}  
```
 


