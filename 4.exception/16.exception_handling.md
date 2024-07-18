## 16.异常处理
# 程序错误
程序的错误大致可以分为三种，分别是语法错误、逻辑错误和运行时错误：
1.语法错误在编译和链接阶段就能发现，只有 100% 符合语法规则的代码才能生成可执行程序。语法错误是最容易发现、最容易定位、最容易排除的错误，程序员最不需要担心的就是这种错误
2.逻辑错误是说我们编写的代码思路有问题，不能够达到最终的目标，这种错误可以通过调试来解决
3.运行时错误是指程序在运行期间发生的错误，例如除数为 0、内存分配失败、数组越界、文件不存在等。C++ 异常机制就是为解决运行时错误而引入的


运行时错误如果放任不管，系统就会执行默认的操作，终止程序运行，也就是我们常说的程序崩溃
一个发生运行时错误的程序：
```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string str = "http://c.biancheng.net";
    char ch1 = str[100];  //下标越界，ch1为垃圾值
    cout<<ch1<<endl;
    char ch2 = str.at(100);  //下标越界，抛出异常
    cout<<ch2<<endl;
    return 0;
}
```
运行代码，在控制台输出 ch1 的值后程序崩溃

# 抛出异常
```c++
char ch1 = str[100];  // [ ] 不会检查下标越界，不会抛出异常
char ch2 = str.at(100);  //  at() 函数执行过程中下标越界，抛出异常
```
也就是说 at() 函数会有抛出异常的动作，才会被捕获

# 捕获异常
我们可以借助 C++ 异常机制来捕获上面的异常，避免程序崩溃。捕获异常的语法为：
```cpp
try{  
    // 可能抛出异常的语句  
}catch(exceptionType variable){  
    // 处理异常的语句  
}
```
try和catch都是 C++ 中的关键字，后跟语句块，不能省略{ }
try 检测语句块有没有异常，一旦有异常抛出不会再执行异常点后面的语句，并且会被后面的 catch 捕获
如果 try 语句块没有检测到异常（没有异常抛出），那么就不会执行 catch 中的语句
catch 关键字后面的exceptionType variable指明了当前 catch 可以处理的异常类型，以及具体的出错信息
修改上面的代码，加入捕获异常的语句：
```c++
#include <iostream>
#include <string>
#include <exception>
using namespace std;

int main(){
    string str = "http://c.biancheng.net";
  
    try{
        char ch1 = str[100];
        cout<<ch1<<endl;
    }catch(exception e){//exceptionType 是异常类型，它指明了当前的 catch 可以处理什么类型的异常；e 是一个变量，用来接收异常信息，当程序抛出异常时，会创建一份数据，这份数据包含了错误信息。
        cout<<"[1]out of bound!"<<endl;
    }

    try{
        char ch2 = str.at(100);
        cout<<ch2<<endl;
    }catch(exception &e){  //exception类位于<exception>头文件中,之所以使用引用，是为了提高效率。如果不使用引用，就要经历一次对象拷贝（要调用拷贝构造函数）的过程。
        cout<<"[2]out of bound!"<<endl;
        cout << "Caught an exception: " << e.what() << endl;//what() 函数返回一个能识别异常的字符串，正如它的名字“what”一样，可以粗略地告诉你这是什么异常。
    }

    return 0;
}
运行结果：
[2]out of bound!
```
检测到异常后程序的执行流会发生跳转，从异常点跳转到 catch 所在的位置，位于异常点之后的、并且在当前 try 块内的语句就都不会再执行了；即使 catch 语句成功地处理了错误，程序的执行流也不会再回退到异常点，所以这些语句永远都没有执行的机会了
执行完 catch 块所包含的代码后，程序会继续执行 catch 块后面的代码，就恢复了正常的执行流
| exception 类的直接派生类 异常                      | 结果  |
| ----------------------------- | ----- |
| logic_error    | 逻辑错误  |
| runtime_error   | 运行时错误 |
| bad_alloc | 使用 new 或 new[ ] 分配内存失败时抛出的异常 |
| bad_typeid  | 使用 typeid 操作一个 NULL 指针，而且该指针是带有虚函数的类，这时抛出 bad_typeid 异常  |
| bad_cast      | 使用 dynamic_cast 转换失败时抛出的异常  |
| ios_base::failure   | io 过程中出现的异常  |
| bad_exception    | 这是个特殊的异常，如果函数的异常列表里声明了 bad_exception 异常，当函数内部抛出了异常列表中没有的异常时，如果调用的 unexpected() 函数中抛出了异常，不论什么类型，都会被替换为 bad_exception 类型  |


| logic_error 的派生类 异常                      | 结果  |
| ----------------------------- | ----- |
| length_error        | 试图生成一个超出该类型最大长度的对象时抛出该异常,例如 vector 的 resize 操作。  |
| domain_error        | 参数的值域错误,主要用在数学函数中,例如使用一个负值调用只能操作非负数的函数。 |
| out_of_range    | 超出有效范围。 |


| runtime_error 的派生类 异常                      | 结果  |
| ----------------------------- | ----- |
| range_error        | 计算结果超出了有意义的值域范围。 |
| overflow_error      | 算术计算上溢。 |
| underflow_error   | 算术计算下溢。 |
