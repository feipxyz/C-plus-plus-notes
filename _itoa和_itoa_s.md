# _itoa 和_itoa_s  

@(c++高级)[itoa, _itoa]

#### 1. _ itoa

```
char*  _itoa ( int value, char * buffer, int radix );
```
它包含三个参数：  
`value` 是要转换的数字；  
`buffer` 是存放转换结果的字符串；
`radix` 是转换所用的基数，2-36。如，2：二进制，10：十进制，16：十六进制

扩展：
`ltoa()` 将长整型值转换为字符串
`ultoa()` 将无符号长整型值转换为字符串

####2.  _itoa_s
是` _itoa` 的安全版本，除了参数和返回值不同，两个函数的行为是相同的，都是将整数转换为字符串。
```cpp
errno_t _itoa_s(int value, char *buffer, size_t sizeInCharacters, int radix);
```
 

`_itoa_s` 比 `_itoa` 多出一个参数：

 `value` 是要转换的数字  
`buffer` 是存放转换结果的字符串  
`sizeInCharacters` 存放转换结果的字符串长度  
`radix` 是转换所用的基数，2-36。如，2：二进制，10：十进制，16：十六进制

 