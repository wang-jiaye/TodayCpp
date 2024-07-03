## 17.模版
C++ 是强类型语言，后来 C++ 开始支持模板，主要目的是：弥补强类型语言“不够灵活”的缺点
### 推出模板的原因
数据结构中每份数据的类型无法提前预测。以链表为例，它的每个节点可以用来存储小数、整数、字符串等，也可以用来存储一名学生、教师、司机等，还可以直接存储二进制数据，这些都是可以的，没有任何限制。而 C++ 又是强类型的，数据的种类受到了严格的限制，这种矛盾是无法调和的。要想解决这个问题，C++ 必须推陈出新，跳出现有规则的限制，开发新的技术，于是模板就诞生了。
模板所支持的类型是宽泛的，没有限制的，我们可以使用任意类型来替换，这种编程方式称为泛型编程。
### 函数模板
语法：
```c++
template <typename 类型参数1 , typename 类型参数2 , ...> 返回值类型  函数名(形参列表){  
    // 函数体
}
```
有四个名字相同，参数列表不同的函数可以使用函数模板合并
```c++
// 交换 int 变量的值
void Swap(int *a, int *b){
    int temp = *a;
    *a = *b;
    *b = temp;
}

// 交换 float 变量的值
void Swap(float *a, float *b){
    float temp = *a;
    *a = *b;
    *b = temp;
}

// 交换 char 变量的值
void Swap(char *a, char *b){
    char temp = *a;
    *a = *b;
    *b = temp;
}

// 交换 bool 变量的值
void Swap(bool *a, bool *b){
    char temp = *a;
    *a = *b;
    *b = temp;
}
```
使用函数模板合并：
```c++
#include <iostream>
using namespace std;

//template是定义函数模板的关键字，它后面紧跟尖括号<>，尖括号包围的是类型参数（也可以说是虚拟的类型，或者说是类型占位符）
//typename是另外一个关键字，用来声明具体的类型参数，这里的类型参数就是 T
//template<typename T>则被称为模板头
template<typename T> void Swap(T *a, T *b){
    T temp = *a;
    *a = *b;
    *b = temp;
}

int main(){
    // 交换 int 变量的值
    int n1 = 100, n2 = 200;
    Swap(&n1, &n2);
    cout<<n1<<", "<<n2<<endl;
   
    // 交换 float 变量的值
    float f1 = 12.5, f2 = 56.93;
    Swap(&f1, &f2);
    cout<<f1<<", "<<f2<<endl;
   
    // 交换 char 变量的值
    char c1 = 'A', c2 = 'B';
    Swap(&c1, &c2);
    cout<<c1<<", "<<c2<<endl;
   
    // 交换 bool 变量的值
    bool b1 = false, b2 = true;
    Swap(&b1, &b2);
    cout<<b1<<", "<<b2<<endl;

    return 0;
}

运行结果：

200, 100  
56.93, 12.5  
B, A  
1, 0
```

### 多个类型参数
```c++
#include <iostream>

template <typename T1, typename T2>
void print_sum(const T1& a, const T2& b) {
    std::cout << "两数之和是: " << a + b << std::endl;
}

int main() {
    int int_num = 1;
    float float_num = 2.5f;
    double double_num = 3.5;

    print_sum(int_num, float_num);    // 两数之和是: 3.5
    print_sum(int_num, double_num);   // 两数之和是: 4.5
    print_sum(float_num, double_num); // 两数之和是: 6
    return 0;
}
```

### 类模板
C++ 除了支持函数模板，还支持类模板 。函数模板中定义的类型参数可以用在函数声明和函数定义中，类模板中定义的类型参数可以用在类声明和类实现中。类模板的目的同样是将数据的类型参数化。
语法：
```c++
template<typename 类型参数1 , typename 类型参数2 , …> class 类名{  
    // TODO:  
};

eg：
template<typename T1, typename T2>  // 这里不能有分号
class Point{
public:
    Point(T1 x, T2 y): m_x(x), m_y(y){ }
public:
    T1 getX() const;  // 获取 x 坐标
    void setX(T1 x);  // 设置 x 坐标
    T2 getY() const;  // 获取 y 坐标
    void setY(T2 y);  // 设置 y 坐标
private:
    T1 m_x;  // x 坐标   借助类模板可以将数据类型参数化，这样就不必定义多个类
    T2 m_y;  // y 坐标
};

```

### 模板类的成员函数
在类外定义成员函数时仍然需要带上模板头
```c++
template<typename 类型参数1 , typename 类型参数2 , …>  
返回值类型 类名<类型参数1 , 类型参数2, ...>::函数名(形参列表){  
    // TODO:  
}

eg：
template<typename T1, typename T2>  //模板头 
T1 Point<T1, T2>::getX() const /*函数头  除了 template 关键字后面要指明类型参数，类名 Point 后面也要带上类型参数*/ {
    return m_x;
}

template<typename T1, typename T2>
void Point<T1, T2>::setX(T1 x){
    m_x = x;
}

template<typename T1, typename T2>
T2 Point<T1, T2>::getY() const{
    return m_y;
}

template<typename T1, typename T2>
void Point<T1, T2>::setY(T2 y){
    m_y = y;
}

```

完整实例：
```c++
#include <iostream>
using namespace std;

template<typename T1, typename T2>  //这里不能有分号
class Point{
public:
    Point(T1 x, T2 y): m_x(x), m_y(y){ }
public:
    T1 getX() const;  //获取x坐标
    void setX(T1 x);  //设置x坐标
    T2 getY() const;  //获取y坐标
    void setY(T2 y);  //设置y坐标
private:
    T1 m_x;  //x坐标
    T2 m_y;  //y坐标
};

template<typename T1, typename T2>  //模板头
T1 Point<T1, T2>::getX() const /*函数头*/ {
    return m_x;
}

template<typename T1, typename T2>
void Point<T1, T2>::setX(T1 x){
    m_x = x;
}

template<typename T1, typename T2>
T2 Point<T1, T2>::getY() const{
    return m_y;
}

template<typename T1, typename T2>
void Point<T1, T2>::setY(T2 y){
    m_y = y;
}

int main(){
    Point<int, int> p1(10, 20);
    cout<<"x="<<p1.getX()<<", y="<<p1.getY()<<endl;
 
    Point<int, char*> p2(10, "东经180度");
    cout<<"x="<<p2.getX()<<", y="<<p2.getY()<<endl;
 
    Point<char*, char*> *p3 = new Point<char*, char*>("东经180度", "北纬210度");
    cout<<"x="<<p3->getX()<<", y="<<p3->getY()<<endl;

    return 0;
}
结果:
x=10, y=20  
x=10, y=东经180度  
x=东经180度, y=北纬210度
```