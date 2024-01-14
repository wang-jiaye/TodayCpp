## 8.指针
每一个变量都有一个内存位置，每一个内存位置都可使用取地址（&）运算符访问的地址，它表示这个变量在内存中的地址
```c++
#include <iostream>
using namespace std;
 
int main ()
{
   int  var1;
   char var2[10];
 
   cout << "var1 变量的地址： ";
   cout << &var1 << endl;
 
   cout << "var2 变量的地址： ";
   cout << &var2 << endl;
 
   return 0;
}
运行结果：
复制代码var1 变量的地址： 0xbfebd5c0
var2 变量的地址： 0xbfebd5b6
```
指针是一个变量，其值为另一个变量的地址，即内存位置的直接地址。就像其他变量或常量一样，必须在使用指针存储其他变量地址之前，对其进行声明。指针变量声明的一般形式为：
在这个语句中，星号是用来指定一个变量是指针。
```c++
int    *ip;    /* 一个整型的指针 */
double *dp;    /* 一个 double 型的指针 */
float  *fp;    /* 一个浮点型的指针 */
char   *ch;    /* 一个字符型的指针 */
```
在这个语句中，星号是用来指定一个变量是指针。所有指针的值的实际数据类型，不管是整型、浮点型、字符型，还是其他的数据类型，都是一个代表内存地址的长的十六进制数。不同数据类型的指针的类型区别是指向的变量的类型不同。

指针的使用：
```c++
#include <iostream>
 
using namespace std;
 
int main ()
{
   int  var = 20;   // 实际变量的声明
   int  *ip = var;        // 指针变量的声明
 
   cout << "变量 var 的值是：";
   cout << var << endl;
 
   // 输出在指针变量中存储的地址
   cout << "指针 ip 指向的地址是：";
   cout << ip << endl;
 
   // 访问指针中地址的值
   cout << "指针 ip 指向的地址存的变量的值是：";
   cout << *ip << endl;
 
   return 0;
}
运行结果：
变量 var 的值是：20
指针 ip 指向的地址是：0xbfc601ac
指针 ip 指向的地址存的变量的值是：20
```

### **空指针**
赋为 NULL 值的指针被称为空指针
```c++
#include <iostream>
using namespace std;

int main ()
{
   int  *ptr = NULL;
   cout << "ptr 的值是 " << ptr ;
   return 0;
}
运行结果：
复制代码ptr 的值是 0
```
内存地址 0 有特别重要的意义，它表明该指针不指向一个可访问的内存位置。空指针可以确保不指向任何对象或函数；而未初始化的指针则可能指向任何地方导致一些异常。


### **野指针**
野指针不是空指针，是一个指向垃圾内存的指针。
形成原因:
- 1.指针变量没有被初始化。
任何指针变量被刚创建时不会被自动初始化为NULL指针，它的缺省值是随机的。所以，指针变量在创建的同时应当被初始化，要么将指针设置为NULL，要么让它指向合法的内存。例如：
```c++
char* p = NULL;  
char* str = (char*)malloc(1024);   
``` 
- 2.指针被free或者delete之后，没有设置为NULL，让人误以为这是一个合法指针。
free和delete只是把指针所指向的内存给释放掉，但并没有把指针本身给清理掉。这时候的指针依然指向原来的位置，只不过这个位置的内存数据已经被毁尸灭迹，此时的这个指针指向的内存就是一个垃圾内存。但是此时的指针由于并不是一个NULL指针（在没有置为NULL的前提下），在做如下指针校验的时候：
```c++
if(p != NULL)  
```
会逃过校验，此时的p不是一个NULL指针，也不指向一个合法的内存块，造成会面程序中指针访问的失败。
- 3.指针操作超越了变量的作用范围。
由于C/C++中指针有++操作，因而在执行该操作的时候，稍有不慎，就容易指针访问越界，访问了一个不该访问的内存，结果程序崩溃
另一种情况是指针指向一个临时变量的引用，当该变量被释放时，此时的指针就变成了一个野指针。


### **指针的自增**
```c++
#include <iostream>
using namespace std;
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
   int  *ptr;
 
   ptr = var; // 指针中的数组地址
   // ptr = &var[MAX-1]; // 指针中最后一个元素的地址
   for (int i = 0; i < MAX; i++)
   {
      cout << "Address of var[" << i << "] = ";
      cout << ptr << endl;
 
      cout << "Value of var[" << i << "] = ";
      cout << *ptr << endl;
 
      ptr++; // 移动到下一个位置
   }
   return 0;
}
运行结果：
Address of var[0] = 0xbfa088b0
Value of var[0] = 10
Address of var[1] = 0xbfa088b4
Value of var[1] = 100
Address of var[2] = 0xbfa088b8
Value of var[2] = 200
```
### **指向指针的指针**
指针的指针就是将指针的地址存放在另一个指针里面
通常，一个指针包含一个变量的地址。当我们定义一个指向指针的指针时，第一个指针包含了第二个指针的地址，第二个指针指向包含实际值的位置
当一个目标值被一个指针间接指向到另一个指针时，访问这个值需要使用两个星号运算符，如下面实例所示：
```c++
#include <iostream>
using namespace std;
 
int main ()
{
    int  var;
    int  *ptr;
    int  **pptr;
 
    var = 3000;
    // 获取 var 的地址
    ptr = &var;
    // 使用运算符 & 获取 ptr 的地址
    pptr = &ptr;
 
    // 使用 pptr 获取值
    cout << "var 值为 :" << var << endl;
    cout << "*ptr 值为:" << *ptr << endl;
    cout << "**pptr 值为:" << **pptr << endl;
 
    return 0;
}
运行结果：
var 值为 :3000
*ptr 值为:3000
**pptr 值为:3000
```
多级间接寻址的应用场景：
- 动态内存分配：使用多级指针可以实现动态内存分配，即分配一个指针数组，用于存储多个指针变量的地址，然后再分配每个指针变量所指向的内存空间。
- 多维数组：在多维数组中，可以使用多级指针来表示二维、三维等多维数组，以便在程序中动态地分配和访问多维数组。
- 数据结构：在数据结构中，可以使用多级指针来表示链表、树等数据结构，以便在程序中动态地添加、删除和修改节点。
- 函数指针：在函数指针中，可以使用多级指针来表示指向函数指针的指针，以便在程序中动态地管理函数指针

### **函数与指针**
可以将指针作为函数参数传入，从而改变这个传入的变量的值
```c++
#include <iostream>
void getSeconds(unsigned long *par)
{
   *par = time( NULL ); // 获取当前的秒数
   return;
}

int main ()
{
   unsigned long sec;
   getSeconds( &sec );
   std::cout << "Number of seconds :" << sec << std::endl ;// 输出实际值
   return 0;
}
运行结果：
Number of seconds : 1695175690


#include <iostream>
using namespace std;
 
void changeval(int *arr, int size) 
{      
  for (int i = 0; i < size; ++i)
  {
    arr[i] = 8;
  }
  return;
}

int main () {  
   int arr[3] = {1, 2, 3};
   changeval( arr, 3 ); 
   cout << "value is: " << arr[0] << endl; 
   return 0;
}
运行结果：
value is: 8
```
和变量一样，函数在内存中有固定的地址。函数的实质也是内存中一块固定的空间。
一个指向函数的指针可以这么写：

int (*funcPtr)();  //函数指针

//或者我们也可以这么写,如果你需要一个静态的函数指针

int (*const funcPtr)();

另外，对于 const int(*funcPtr),意思是这个指针指向的函数的返回值是常量
把一个函数赋值给函数指针
```c++
int foo()
{
    return 5;
}

int hoo(int x)
{
    return 4;
};

double poo(){
    return 4.6;
};

int goo()
{
    return 6;
}
 
int main()
{
    int (*funcPtr)() = foo; // funcPtr 现在指向了函数foo
    funcPtr = goo; // funcPtr 现在又指向了函数goo
 //但是千万不要写成funcPtr = goo();这是把goo的返回值赋值给了funcPtr

    int (*funcPtr1)() = foo; // 可以
    int (*funcPtr2)() = poo; // 错误！返回值不匹配！
    double (*funcPtr4)() = poo; // 可以
    funcPtr1 = hoo; // 错误，因为参数不匹配，funcPtr1只能指向不含参数的函数，而hoo含有int型的参数
    int (*funcPtr3)(int) = hoo; // 可以，所以应该这么写

    funcPtr1();// 通过函数指针调用函数
    funcPtr3(5);
    return 0;
}
```
把函数作为参数传入另一个函数
```c++
#include <iostream>
int add(int a, int b){
    return a+b;
}
int sub(int a, int b){
    return a-b;
}
void func(int e, int d, int(*f)(int a, int b)){ // 这里才是我想说的，
// 传入了一个int型，双参数，返回值为int的函数
    std::cout<<f(e,d)<<std::endl;
}
int main()
{
    func(2,3,add);
    func(2,3,sub);
    return 0;
}
```


## 9.引用
引用的本质是一个已存在变量的别名,为了减少临时变量的copy
```c++
#include <iostream>
using namespace std;
 
int main ()
{
   // 声明简单的变量
   int i = 5;
   double d = 0.7; 
   // 声明引用变量
   int&    r = i;
   double& s = d;
 
   cout << "Value of i : " << i << endl;
   cout << "Value of i reference : " << r  << endl;

   cout << "Value of d : " << d << endl;
   cout << "Value of d reference : " << s  << endl;
   return 0;
}
运行结果：
Value of i : 5
Value of i reference : 5
Value of d : 11.7
Value of d reference : 11.7
```

### **引用做为函数参数**
```c++
#include <iostream>
using namespace std;
 
void swap(int& x, int& y)
{
   int temp;
   temp = x; /* 保存地址 x 的值 */
   x = y;    /* 把 y 赋值给 x */
   y = temp; /* 把 x 赋值给 y  */
}
 
int main ()
{
   // 局部变量声明
   int a = 100;
   int b = 200;
   cout << "交换前，a 的值：" << a << endl;
   cout << "交换前，b 的值：" << b << endl;
 
   /* 调用函数来交换值 */
   swap(a, b);
 
   cout << "交换后，a 的值：" << a << endl;
   cout << "交换后，b 的值：" << b << endl;
   return 0;
}
```

**指针与引用的区别：**
- 指针是可以独立存在的; 但是引用不行
- 引用必须要进行初始化，指针没有必要
- 指针可以设置为NULL， 但是引用不行
- 引用一旦进行初始化之后，不会再改变其指向；但指针可以

**指针与引用的使用场景：**
- 函数参数传递：在函数参数传递中，如果需要修改参数的值，则可以使用指针或引用类型来传递参数。如果参数可以为空，则可以使用指针类型。如果参数不能为空，则可以使用引用类型。例如：
```c++
// 使用指针传递参数
void func(int* p) {
    if(p){
        *p = 10;
    }
}

// 使用引用传递参数
void func(int& r) {
    r = 10;
}
```

- 动态内存分配：在动态内存分配中，需要使用指针类型来存储分配的内存地址，以便后续访问内存中的数据。例如：
```c++
// 动态分配内存，返回指针
int* p = new int[10];

// 访问内存中的数据
p[0] = 1;
```

- 数据结构：在数据结构中，需要使用指针类型来表示数据结构中的节点，以便在程序中动态地添加、删除和修改节点。例如：
```c++
// 定义链表节点
struct Node {
    int val;
    Node* next;
};

// 动态分配节点，返回指针
Node* p = new Node;
// 修改节点的值和指针
p->val = 1;
p->next = nullptr;
```

- 运算符重载：在运算符重载中，需要使用引用类型来实现运算符的重载，以便更加直观和安全地访问数据。例如：
```c++
// 定义向量类
class Vector {
public:
    // 重载下标运算符
    int& operator[](int i) {
        return data[i];
    }
private:
    int data[10];
};

// 使用引用访问向量元素
Vector v;
v[0] = 1;
```

[C++左右值引用](https://zhuanlan.zhihu.com/p/335994370)