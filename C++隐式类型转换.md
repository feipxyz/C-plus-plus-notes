# C++隐式类型转换
@(c++高级)[隐式类型转换, c++]

普通类型的转换顺序：隐式把`char` $\rightarrow$ `int`和从 `short` $\rightarrow$ `double`。转换可能会导致数据的丢失。  

自定义类型：有两种函数可以进行隐式转换：
- **单参数构造函数**   
- **隐式类型转换符**  

**自定义类型可以用函数前+ `explicit` 关键字，防止转换**

`operator T ()`，其中`T`是一个类型，比如下面这个例子
```
class A
{
public:
   operator   B*() { return this->b_; } 
   operator const   B*() { return this->b_; }
   operator   B&() { return *this->b_; }
private:
   B* b_;
};

A a;
```
当 `if(a)` ，编译时，它转换成`if(a.operator B*())`，其实也就是相当于 `if(a.b_)`

代码如下
```cpp
#include<iostream>
#include <string>
#include <sstream>

using namespace std;

class things
{
public:
	things(const std::string &name = "") :
		m_name(name), height(0), weight(10){}
	int CompareTo(const things & other) 
	{
		if (other.m_name == this->m_name)
		{
			return true;
		}
		else
		{
			return false;
		}
	};
	std::string m_name;
	int height;
	int weight;
};

struct Month
{
	Month(int m) : val(m) {};
	int val;
};

struct Day
{
	explicit Day(int d) : val(d) {};
	int val;
};

int DateMonth(const Month& a)
{
	return a.val;
}

int DateDay(const Day& a)
{
	return a.val;
}

class A 
{
public:
	A operator +(A& oa){ A a; a.num = oa.num + num; return a; }
	operator int(){ return num; };  // 在需要情况下， A对象可以转成int类型对象。  
	int num;
};

class FuncObj
{
public:
	FuncObj(int n) : _n(n) {
		cout << "constructor" << endl;
	}

	bool operator()(int v) {
		cout << "operator overload" << endl;
		return v > _n;
	}

	operator string() {
		cout << "type convert" << endl;
		stringstream sstr;
		sstr << _n;
		return sstr.str();
	}

	int _n;
};


int main()
{
	/*things*/
	things a;
	std::string nm = "book";
	int result = a.CompareTo(nm); //调用隐式转换
	std::cout << result << endl;

	/*Month*/
	cout << "month:" << DateMonth(2) << endl; //调用隐式转换

	/*Day*/
	//cout << "day:" << DateDay(2) << endl;  //编译不过去
	cout << "day:" << DateDay(Day(2)) << endl; //必须这样

	/*operator*/
	A a1;
	a1.num = 10;
	A a2;
	a2.num = 20;
	A a3;
	a3 = a1 + a2;
	cout << "a3: " << a3.num << endl;
	cout << "a3 + 12 = " << a3 + 12 << endl; //调用隐式转换
	 
	int kk;
	std::cin >> kk;

	return 0;
}
```