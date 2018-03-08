# c++ explicit的作用

@(c++高级)[c++, explicit]

C++中的`explicit`关键字**只能用于修饰只有一个参数的类构造函数, 它的作用是表明该构造函数是显示的, 而非隐式的**。 跟它相对应的另一个关键字是implicit，意思是隐藏的，**类构造函数默认情况下即声明为implicit(隐式)**.

那么显示声明的构造函数和隐式声明的有什么区别呢? 我们来看下面的例子:
```c++
 class CxString  // 没有使用explicit关键字的类声明, 即默认为隐式声明  
 {  
 public:  
     char *_pstr;  
     int _size;  
     CxString(int size)  
     {  
         _size = size;                // string的预设大小  
         _pstr = malloc(size + 1);    // 分配string的内存  
         memset(_pstr, 0, size + 1);  
     }  
     CxString(const char *p)  
     {  
         int size = strlen(p);  
         _pstr = malloc(size + 1);    // 分配string的内存  
         strcpy(_pstr, p);            // 复制字符串  
         _size = strlen(_pstr);  
     }  
     // 析构函数这里不讨论, 省略...  
 };  
   
     // 下面是调用:  
   
     CxString string1(24);     // 这样是OK的, 为CxString预分配24字节的大小的内存  
     CxString string2 = 10;    // 这样是OK的, 为CxString预分配10字节的大小的内存  
     CxString string3;         // 这样是不行的, 因为没有默认构造函数, 错误为: “CxString”: 没有合适的默认构造函数可用  
     CxString string4("aaaa"); // 这样是OK的  
     CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
     CxString string6 = 'c';   // 这样也是OK的, 其实调用的是CxString(int size), 且size等于'c'的ascii码  
     string1 = 2;              // 这样也是OK的, 为CxString预分配2字节的大小的内存  
     string2 = 3;              // 这样也是OK的, 为CxString预分配3字节的大小的内存  
     string3 = string1;        // 这样也是OK的, 至少编译是没问题的, 但是如果析构函数里用free释放_pstr内存指针的时候可能会报错, 完整的代码必须重载运算符"=", 并在其中处理内存释放  
```

explicit关键字的作用就是防止类构造函数的隐式自动转换.

上面也已经说过了, explicit关键字只对有一个参数的类构造函数有效, 如果类构造函数参数大于或等于两个时, 是不会产生隐式转换的, 所以explicit关键字也就无效了. 例如: 

```

class CxString  // explicit关键字在类构造函数参数大于或等于两个时无效  
{  
public:  
    char *_pstr;  
    int _age;  
    int _size;  
    explicit CxString(int age, int size)  
    {  
        _age = age;  
        _size = size;  
        // 代码同上, 省略...  
    }  
    CxString(const char *p)  
    {  
        // 代码同上, 省略...  
    }  
};  
  
    // 这个时候有没有explicit关键字都是一样的  
```

但是, 也有一个例外, 就是当除了第一个参数以外的其他参数都有默认值的时候, explicit关键字依然有效, 此时, 当调用构造函数时只传入一个参数, 等效于只有一个参数的类构造函数, 例子如下:
```

    class CxString  // 使用关键字explicit声明  
    {  
    public:  
        int _age;  
        int _size;  
        explicit CxString(int age, int size = 0)  
        {  
            _age = age;  
            _size = size;  
            // 代码同上, 省略...  
        }  
        CxString(const char *p)  
        {  
            // 代码同上, 省略...  
        }  
    };  
      
        // 下面是调用:  
      
        CxString string1(24);     // 这样是OK的  
        CxString string2 = 10;    // 这样是不行的, 因为explicit关键字取消了隐式转换  
        CxString string3;         // 这样是不行的, 因为没有默认构造函数  
        string1 = 2;              // 这样也是不行的, 因为取消了隐式转换  
        string2 = 3;              // 这样也是不行的, 因为取消了隐式转换  
        string3 = string1;        // 这样也是不行的, 因为取消了隐式转换, 除非类实现操作符"="的重载  

```


[原文链接](https://www.cnblogs.com/ymy124/p/3632634.html)


