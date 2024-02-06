
## 10.数组
数组的申明及初始化：
```c++
string strarr[2] = {"a", "b"};//数组的元素类型  数组变量名 数组大小
int intarr[4] = {1, 2, 3, 4};//数组大小必须大于等于元素个数
```

**多维数组**
声明及初始化
```c++
int a[2][3] = {  
 {0, 1, 2} ,   /*  初始化索引号为 0 的行 */
 {3, 4, 5}  /*  初始化索引号为 1 的行 */
};

访问
int val = a[1][2];//5
int val = a[0][1];//1
```

**数组与指针**
可以使用指针指向数组后来访问或者改变数组的值
```c++
#include <iostream>
using namespace std;
 
int main ()
{
   // 带有 5 个元素的双精度浮点型数组
   double Aarray[5] = {10.0, 20.0, 30.1, 5.0, 5.1};
   double *p = Aarray;//表示把第一个元素的地址存储在 p 中，接下来就可以使用 *p、*(p+1)、*(p+2) 等来访问数组元素
 
   // 输出数组中每个元素的值
   cout << "使用指针的数组值 " << endl; 
   for ( int i = 0; i < 5; i++ )
   {
       cout << "*(p + " << i << ") : ";
       cout << *(p + i) << endl;
   }
   return 0;
}

运行结果：
使用指针的数组值 
*(p + 0) : 10
*(p + 1) : 20
*(p + 2) : 30.1
*(p + 3) : 5
*(p + 4) : 5.1
```

**数组传入函数**
将一个数组传入函数
```c++
#include <iostream>
using namespace std;

double getAverage(int *arr, int size)
{
  int sum = 0;       
  double avg;
  for (int i = 0; i < size; ++i)
  {
    sum += arr[i];
  }
  avg = double(sum) / size;
  return avg;
}

int main ()
{
   int Aarray[3] = {9, 20, 40}; // 带有3个元素的整型数组
   double avg;
   avg = getAverage( Aarray, 3 ); // 传递一个指向数组的指针作为参数
   cout << "平均值是：" << avg << endl;  // 输出返回值
   return 0;
}
```
**动态分配数组**

在函数中动态分配一个数组，并使用指针返回
```c++
#include <iostream>
using namespace std;

int* createArray(int size) {
    int* arr = new int[size];//动态分配数组
    for (int i = 0; i < size; i++) {
        arr[i] = i + 1;
    }
    return arr;
}

int main() {
    int* myArray = createArray(5); // 调用返回数组的函数，得到一个指向数组的指针
    for (int i = 0; i < 5; i++) {
        cout << myArray[i] << " ";
    }
    cout << endl;
    delete[] myArray; // 释放内存,在函数内部动态分配的数组在函数执行结束时不会自动释放，所以需要调用函数的代码负责释放返回的数组
    return 0;
}

运行结果：
复制代码1 2 3 4 5 
```