## 3.常量
在 C++ 中，有两种定义常量的方式：

第一种是使用 #define 预处理器。
第二种是使用 const 关键字。

define 预处理器
下面是使用 #define 预处理器定义常量的形式：
```c++
#define identifier value
```
具体请看下面的实例：
```c++
#include <iostream>
using namespace std;
 
#define LENGTH 10   
#define WIDTH  5
#define NEWLINE '\n'
 
int main()
{
 
   int area;
   area = LENGTH * WIDTH;
   cout << area;
   cout << NEWLINE;
}
```
const 关键字
还可以使用 const 前缀声明指定类型的常量，如下所示：
```c++
const type variable = value;
```
具体请看下面的实例：
```c++
#include <iostream>
using namespace std;
 
int main()
{
   const int  LENGTH = 10;
   const int  WIDTH  = 5;
   const char NEWLINE = '\n';
   int area;  
   
   area = LENGTH * WIDTH;
   cout << area;
   cout << NEWLINE;
}
```

## 4.类型限定符
类型限定符提供了变量的额外信息，用于在定义变量或函数时改变它们的默认行为的关键字。
|  限定符   | 含义  |
|  ----  | ----  |
| const  | 定义常量，表示该变量的值不能被修改。 |
| volatile  | 告诉该变量的值可能会被程序以外的因素改变，如硬件或其他线程 |
| mutable  | 表示类中的成员变量可以在 const 成员函数中被修改 |
| static  | 用于定义静态变量，表示该变量的作用域仅限于当前文件或当前函数内，static 修饰的静态局部变量只执行初始化一次，延长了局部变量的生命周期直到程序运行结束以后才释放。 |
| register  | 用于定义寄存器变量，表示该变量被频繁使用，可以存储在CPU的寄存器中，以提高程序的运行效率。 |
const 实例
```c++
const int NUM = 10; // 定义常量 NUM，其值不可修改
const int* ptr = &NUM; // 定义指向常量的指针，指针所指的值不可修改
int const* ptr2 = &NUM; // 和上面一行等价
```
volatile 实例
```c++
volatile int num = 20; // 定义变量 num，其值可能会在未知的时间被改变
```
mutable 实例
```c++
class Example {
public:
    int get_value() const {
        return value_; // const 关键字表示该成员函数不会修改对象中的数据成员
    }
    void set_value(int value) const {
        value_ = value; // mutable 关键字允许在 const 成员函数中修改成员变量
    }
private:
    mutable int value_;
};
```
static 实例
```c++
void example_function() {
    static int count = 0; // static 关键字使变量 count 存储在程序生命周期内都存在
    count++;
}
```
register 实例
```c++
void example_function(register int num) {
    // register 关键字建议编译器将变量 num 存储在寄存器中
    // 以提高程序执行速度
    // 但是实际上是否会存储在寄存器中由编译器决定
}
```