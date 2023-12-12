
## 2.类型转换
隐式类型转换
例如将一个 float 类型的值 42.5 转换为 int 类型，由于 int 类型不支持小数部分，因此在进行转换时，小数部分会被截断，只保留整数部分
```c++
float f = 42.5f;
int i = int(f);
```

显式类型转换
C++ 风格的类型转换：使用 static_cast、dynamic_cast、const_cast 或 reinterpret_cast 进行类型转换。例如：
静态转换static_cast是将一种数据类型的值强制转换为另一种数据类型的值
```c++
int i = 42;
float f = static_cast<float>(i);
```
动态转换dynamic_cast通常用于将一个基类指针或引用转换为派生类指针或引用
```c++
class Base {};
class Derived : public Base {};
Base* ptr_base = new Derived;
Derived* ptr_derived = dynamic_cast<Derived*>(ptr_base); // 将基类指针转换为派生类指针
```
常量转换const_cast用于将 const 类型的对象转换为非 const 类型的对象
```c++
const int i = 10;
int& r = const_cast<int&>(i); // 常量转换，将const int转换为int
```
重新解释转换reinterpret_cast将一个数据类型的值重新解释为另一个数据类型的值，通常用于在不同的数据类型之间进行转换，不安全不推荐
```c++
int i = 10;
float f = reinterpret_cast<float&>(i); // 重新解释将int类型转换为float类型
```