# c++ 基础
## 1.数据类型
首先让我们来看几个常见的数据类型，包括布尔型bool，字符型char，整型int
|  类型   | 关键字  |
|  ----  | ----  |
| 布尔型  | bool |
| 字符型  | char |
| 整型  | int |
| 浮点型  | float |
| 双浮点型  | double |


下表显示了各种数据类型在内存中存储值时需要占用的内存，以及该类型的变量所能存储的最大值和最小值
|  类型   | 大小  | 表示范围 |
|  ----  | ----  | ----  |
| char  | 1byte | -128 到 127 |
| unsigned char  | 1byte | 0 到 255 |
| int  | 4byte | -2147483648 到 2147483647 |
| unsigned int  | 4byte | 0 到 4294967295 |
| short int  | 2byte | -32768 到 32767 |
| unsigned short int  | 2byte | 0 到 65,535 |
| long  | 8byte | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807 |
| unsigned long  | 8byte | 0 到 18,446,744,073,709,551,615 |
| float  | 4byte | ~7 个数字 |
| double  | 8byte | ~15 个数字 |

可以发现那对于这些基本类型呢，都可以有对应的修饰符来对其进行进一步的描述

-  signed：表示变量可以存储负数。对于整型变量来说，signed 可以省略，因为整型变量默认为有符号类型

- unsigned：表示变量不能存储负数。对于整型变量来说，unsigned 可以将变量范围扩大一倍

- short：表示变量的范围比 int 更小。

- long：表示变量的范围比 int 更大。

### **uint8_t / uint16_t / uint32_t /uint64_t 是什么数据类型?**

typedef用法
- typedef定义一种类型的别名，但是与#define相比不只是简单的宏替换。
```c++
typedef char * pStr1;  
#define pStr2 char *;  
pStr1 s1, s2;  
pStr2 s3, s4;  
```

- typedef可以用来简化代码：
```c++
原声明：int *(*a[5])(int, char*);
变量名为a，直接用一个新别名pFun替换a就可以了：
typedef int *(*pFun)(int, char*); 

原声明的最简化版：
pFun a[5];
```
- typedef可以用来简化代码：
用typedef来定义与平台无关的类型。
比如定义一个叫 HEI 的浮点类型，在目标平台一上，让它表示最高精度的类型为：
```c++
typedef long double HEI; 
在不支持 long double 的平台二上，改为：
typedef double HEI; 
在连 double 都不支持的平台三上，改为：
typedef float HEI; 
也就是说，当跨平台时，只要改下 typedef 本身就行，不用对其他源码做任何修改。
```

然后我们会在/usr/include/stdint.h文件中发现
```c++
typedef signed char             int8_t;
typedef short int               int16_t;
typedef int                     int32_t;
# if __WORDSIZE == 64
typedef long int                int64_t;
# else
__extension__
typedef long long int           int64_t;
# endif
#endif

/* Unsigned.  */
typedef unsigned char           uint8_t;
typedef unsigned short int      uint16_t;
#ifndef __uint32_t_defined
typedef unsigned int            uint32_t;
# define __uint32_t_defined
#endif
#if __WORDSIZE == 64
typedef unsigned long int       uint64_t;
#else
__extension__
typedef unsigned long long int  uint64_t;
```
uint8_t 实际是一个 char。（typedef unsigned char uint8_t;）输出 uint8_t 类型的变量实际输出的是其对应的字符, 而不是真实数字。
```c++
#include <iostream>
#include <cstdint>

using namespace std;

int main() {
  uint8_t a = 65;
  cout << a << endl;
}
输出：A
```


