# std::pair
@(c++高级)[pair]

`pair` 实质上是一个结构体，其主要的两个成员变量是 `first` 和 `second`。

#### 1. pair的应用
- `std::pair` 主要的作用是将两个数据组合成一个数据，两个数据可以是同一类型或者不同类型。例如   
```
std::pair<int,float>
std::pair<double,double>
```
- 当一个函数需要返回2个数据的时候，可以选择 `pair`。  

#### 2. make_pair函数

初始化一个 `pair` 可以使用构造函数，也可以使用 `std::make_pair` 函数，`make_pair` 函数的定义如下：
```cpp
template pair make_pair(T1 a, T2 b) 
{ 
	return pair(a, b); 
}
```

一般 `make_pair`都使用在需要 `pair` 做参数的位置，可以直接调用 `make_pair` 生成 `pair` 对象。 另一个使用的方面就是 `pair` 可以接受隐式的类型转换，这样可以获得更高的灵活度。但是这样会出现如下问题：例如有如下两个定义：
```cpp
std::pair<int, float>(1, 1.1);
std::make_pair(1, 1.1);
```

其中第一个的 `second` 变量是 `float` 类型，而 `make_pair` 函数会将 `second` 变量都转换成 `double` 类型。这个问题在编程是需要引起注意。

