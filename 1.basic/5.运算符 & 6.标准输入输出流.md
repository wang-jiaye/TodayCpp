## 5.运算符
下表显示了 C++ 支持的算术运算符。
假设变量 A 的值为 10，变量 B 的值为 21，则：
|  类型   | 功能  | 
|  ----  | ----  |
| +  | 相加A + B |
| -  | 相减A - B |
| *  | 相乘A * B |
| /  | 取整B / A |
| %  | 取余B % A |
| ++  | 自增运算符，变量数值增加1 |
| --  | 自减运算符，变量数值减少1 |

下表显示了 C++ 支持的关系逻辑运算符。
假设变量 A 的值为 1，变量 B 的值为 0，则：
|  类型   | 功能  | 
|  ----  | ----  |
| && | 逻辑与。如果两个操作数都 true，则条件为 true。 |
| || | 逻辑或。如果两个操作数中有任意一个 true，则条件为 true。 |
| ！ | 逻辑非。如果条件为 true 则逻辑非将使其为 false。 |

下表显示了 C++ 支持的其他重要运算符。
|  类型   | 功能  | 
|  ----  | ----  |
| sizeof  | 运算符返回变量所占的字节大小。 |
| Condition ? A : B  | 三目运算符，问号前为真则为A，否则为B。|
| ::  | 用于直接访问命名空间、类、结构体、共用体或枚举类型的成员或静态成员 |
| & | 由变量获取到它的地址。|
| * | 由地址获取到地址指向的变量的值。 |

```c++
int main ()
{
   int  var;
   int  *ptr; // * 运算符也可以用来表示一个指针
   int  val;
   var = 3000;

   ptr = &var; // 获取变量 var 的地址，赋值给 ptr
   val = *ptr; // 获取地址 ptr 指向的变量 var 的值
   cout << "Value of var :" << var << endl;
   cout << "Value of ptr :" << ptr << endl;
   cout << "Value of val :" << val << endl;
   return 0;
}
```



## 6.标准输入输出流
<iostream>	该文件定义了 cin、cout、cerr 和 clog 对象，分别对应于标准输入流、标准输出流、非缓冲标准错误流和缓冲标准错误流。

**输出流** cout
```c++
#include <iostream>
using namespace std;
 
int main( )
{
   string str = "Hello C++";
   cout << "Value of str is : " << str << endl;
}
```

**输入流** cin
```c++
#include <iostream>
using namespace std;
 
int main( )
{
   char name[50];
   cout << "请输入您的名称： ";
   cin >> name;
   cout << "您的名称是： " << name << endl;
}
```

**错误流** cerr
未缓冲的标准错误流 （cerr）：C++ cerr 是用于输出错误的标准错误流。这也是 iostream 类的一个实例。由于 cerr 在 C++是无缓冲的，因此在需要立即显示错误消息时使用。它没有任何缓冲区来存储错误消息并在以后显示它。cerr和cout之间的主要区别在于当您想使用“cout”重定向输出时，如果您使用“cerr”，则错误不会存储在文件中（这就是未缓冲的意思.它不能存储消息）。
```c++
#include <iostream>
using namespace std;
 
int main( )
{
   char str[] = "Unable to read....";
   cerr << "Error message : " << str << endl;
}
```

**日志流** clog
缓冲标准错误流（clog）：这也是ostream类的一个实例，用于显示错误，但与cerr不同，错误首先插入缓冲区并存储在缓冲区中。在使用 flush错误消息也将显示在屏幕上。
```c++
#include <iostream>
using namespace std;
 
int main( )
{
   char str[] = "able to read....";
   clog << "Log message : " << str << endl;
}
```
