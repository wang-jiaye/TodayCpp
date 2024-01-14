## 7.日期和时间
### 1. C/C++ 中关于日期和时间的重要函数
 |  函数类型   | 功能  | 
|  ----  | ----  |
| time_t now = time(0);  | 返回系统的当前日历时间，自 1970 年 1 月 1 日以来经过的秒数。如果系统没有时间，则返回 -1 |
| char * strnow = ctime(&now);  | 把得到的now转换成字符串形式， day month year hours:minutes:seconds year|
| double difftime ( time_t time2, time_t time1 );  | 该函数返回 time1 和 time2 之间相差的秒数 |
| size_t strftime(char *str, size_t maxsize, const char *format, const struct tm *timeptr) | 该函数可用于格式化日期和时间为指定的格式 |

```c++
#include <iostream>
#include <ctime>
using namespace std;
 
int main()
{
   //1. 基于当前系统的当前日期/时间
   time_t now = time(0);  
   // 把 now 转换为字符串形式
   char* dt = ctime(&now);
   cout << "本地日期和时间：" << dt << endl;

   //2.自定义时间格式
   time_t rawtime;
   struct tm *info;
   char buffer[50];
   time( &rawtime );
   info = localtime( &rawtime );
   strftime(buffer, 50, "%Y-%m-%d %H:%M:%S", info);
   printf("格式化的日期 & 时间 : %s\n", buffer );
}
输出结果：
本地日期和时间：Fri Sep 15 06:44:51 2023
输出结果：
格式化的日期 & 时间 : 2023-09-15 06:45:5
```

### 2. 使用C++11的标准库

C++ 标准库提供了一些函数来获取当前时间。其中最常用的是std::chrono::system_clock::now() 函数。它返回当前时间点的时间戳，以自 1970 年 1 月 1 日 00:00:00 UTC 起经过的秒数和纳秒数的组合形式表示。

以下是一个使用 std::chrono::system_clock::now() 函数获取当前时间戳并打印的示例代码：
```c++
#include <iostream>
#include <chrono>
 
int main()
{
    auto now = std::chrono::system_clock::now();
    auto timestamp = std::chrono::duration_cast<std::chrono::seconds>(now.time_since_epoch()).count();
 
    std::cout << "Current timestamp: " << timestamp << std::endl;
 
    return 0;
}
```