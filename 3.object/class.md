## 11.类和对象
C++ 在 C 语言的基础上增加了面向对象编程,类是用户自定义的类型，是 C++ 的核心特性。类可以被看作是一种模板，可以用来创建具有相同属性和行为的多个对象。

**定义类**
```c++
class Student{ //class是 C++ 中新增的关键字，专门用来定义类
public: // public是 C++ 的新增关键字，它只能用在类的定义中，表示类的成员变量或成员函数具有“公有”的访问权限
    //成员变量
    char *name;
    int age;
    float score;

    //成员函数
    void say(){
        cout<<name<<"的年龄是"<<age<<"，成绩是"<<score<<endl;
    }
}; //类定义的最后有一个分号;，它表示类定义结束了，不能省略
```
类只是一个模板（Template），编译后不占用内存空间，所以在定义类时不能对成员变量进行初始化，因为没有地方存储数据。只有在创建对象以后才会给成员变量分配内存，这个时候就可以赋值了。
关于类的访问权限C++通过 public、protected、private 三个关键字来控制成员变量和成员函数的访问权限，它们分别表示公有的、受保护的、私有的，被称为成员访问限定符。所谓访问权限，就是能不能使用该类中的成员。

在类的内部，无论成员被声明为 public、protected 还是 private，都是可以互相访问的，没有访问权限的限制
在类的外部，只能通过对象访问成员，并且通过对象只能访问 public 属性的成员，不能访问 private、protected 属性的成员
protected（受保护）成员变量或函数与私有成员private十分相似，但有一点不同，protected（受保护）成员在派生类（即子类）中是可访问的。

**创建类对象**
有了 Student 类后，就可以通过它来创建对象了，例如：
```cpp
Student stu;  // 创建对象 创建的对象 stu 在栈上分配内存，需要使用&获取它的地址
Student *pStu = new Student; // 使用 new 关键字创建对象 使用 new 在堆上创建出来的对象是匿名的，没法直接使用，必须要用一个指针指向它，再借助指针来访问它的成员变量或成员函数
```
两者区别是：
1.通过类创建的对象这个变量是在栈内存
2.通过 new 关键字创建的对象这个变量是在堆内存
3.new的对象需使用对象指针访问
有了对象指针后，可以通过箭头->来访问对象的成员变量和成员函数，这和通过结构体指针来访问它的成员类似：
```cpp
pStu -> name = "小明";
pStu -> age = 15;
pStu -> score = 92.5f;
pStu -> say();
```
4.需要delete 及时释放内存
栈内存是程序自动管理的，不能使用 delete 删除在栈上创建的对象；堆内存由程序员管理，对象使用完毕后可以通过 delete 删除。在实际开发中，new 和 delete 往往成对出现，以保证及时删除不再使用的对象，防止无用内存堆积
```cpp
#include <iostream>
using namespace std;

class Student{
public:
    char *name;
    int age;
    float score;

    void say(){
        cout<<name<<"的年龄是"<<age<<"，成绩是"<<score<<endl;
    }
};

int main(){
    Student *pStu = new Student;
    pStu -> name = "小明";
    pStu -> age = 15;
    pStu -> score = 92.5f;
    pStu -> say();
    delete pStu;  // 删除对象
    return 0;
}
```
[C++11新特性] 智能指针详解 
https://www.cnblogs.com/linuxAndMcu/p/10409723.html
C++ 标准库中也提供了一些智能指针（如 unique_ptr、shared_ptr等），它们可以帮助程序员更加方便地管理内存，避免出现内存泄漏和悬空指针等问题。这些智能指针使用了 RAII（资源获取即初始化）的技术，可以在对象生命周期结束时自动释放内存。


**类对象访问**
创建对象以后，可以使用点运算符 . 来访问成员变量和成员函数，这和通过结构体变量来访问它的成员类似，如下所示：
```c++
#include <iostream>
using namespace std;

// 类通常定义在函数外面
class Student{
public:
    // 成员变量
    char *name;
    int age;
    float score;
    // 成员函数
    void say(){
        cout<<name<<"的年龄是"<<age<<"，成绩是"<<score<<endl;
    }
};

int main(){
    Student stu; // 创建对象
    stu.name = "小明";
    stu.age = 15;
    stu.score = 92.5f;
    stu.say(); // 调用类的成员函数
    return 0;
}
运行结果：
复制代码小明的年龄是15，成绩是92.5
```

```c++
DCos &DCos::Instance() {
  static DCos inst; //之所以需要使用一个static的类对象，是为了在整套代码里面用同一个对象。单例模式，如果不是static的话那每调一次就会创建一个对象。
  return inst; 
}
```
标准单例：
```c++
局部静态变量不仅只会初始化一次，而且还是线程安全的。

class Singleton{
public:
    // 注意返回的是引用。
    static Singleton& getInstance(){
        static Singleton m_instance;  //局部静态变量
        return m_instance;
    }
private:
    Singleton(); //私有构造函数，不允许使用者自己生成对象
    Singleton(const Singleton& other);
};
这种单例被称为Meyers' Singleton 。这种方法很简洁，也很完美，但是注意：

gcc 4.0之后的编译器支持这种写法。
C++11及以后的版本（如C++14）的多线程下，正确。
C++11之前不能这么写。
但是现在都19年了,新项目一般都支持了c++11了。
```

**C++中explicit关键字用法**
[C++11新特性] 智能指针详解 
https://www.cnblogs.com/linuxAndMcu/p/10409723.html

[C++中explicit关键字用法]
https://blog.csdn.net/dcrmg/article/details/75451678


**C++中类的成员变量及成员函数**
关于类的申明及定义，推荐使用pimpl的实现方式。
首先在实际的头文件中完成对应类的申明：


C++类声明中的关键字：default、delete、constexpr 和 explicit
https://zhuanlan.zhihu.com/p/374042021


**C++类声明中的noexcept：**
https://www.cnblogs.com/RioTian/p/15115387.html
noexcept 紧跟在函数的参数列表后面，它只用来表明两种状态："不抛异常" 和 "抛异常"。
```c++
void func_not_throw() noexcept; // 保证不抛出异常
void func_not_throw() noexcept(true); // 和上式一个意思

void func_throw() noexcept(false); // 可能会抛出异常
void func_throw(); // 和上式一个意思，若不显示说明，默认是会抛出异常（除了析构函数，详见下面）
```
对于一个函数而言，
noexcept 说明符要么出现在该函数的所有声明语句和定义语句，要么一次也不出现。
函数指针及该指针所指的函数必须具有一致的异常说明。
在 typedef 或类型别名中则不能出现 noexcept。
在成员函数中，noexcept 说明符需要跟在 const 及引用限定符之后，而在 final、override 或虚函数的 =0 之前。
如果一个虚函数承诺了它不会抛出异常，则后续派生的虚函数也必须做出同样的承诺；与之相反，如果基类的虚函数允许抛出异常，则派生类的虚函数既可以抛出异常，也可以不允许抛出异常。

- **显示指定异常说明符的益处**
1.从语义上，noexcept 对于程序员之间的交流是有利的，就像 const 限定符一样。
2.显示指定 noexcept 的函数，编译器会进行优化
因为在调用 noexcept 函数时不需要记录 exception handler，所以编译器可以生成更高效的二进制码（编译器是否优化不一定，但理论上 noexcept 给了编译器更多优化的机会）。另外编译器在编译一个 noexcept(false) 的函数时可能会生成很多冗余的代码，这些代码虽然只在出错的时候执行，但还是会对 Instruction Cache 造成影响，进而影响程序整体的性能。

**C++类的静态变量**
static 静态成员变量属于类，不属于某个具体的对象，即使创建多个对象，也只为静态成员变量分配一份内存，所有对象使用的都是这份内存中的数据。当某个对象修改了这个静态成员变量，也会影响到其他对象。类共享的
以Student 类为例，如果我们想知道班级中共有多少名学生，就可以设置一份共享的变量，每次创建对象时让该变量加 1
```cpp
class Student{
public:
    Student(char *name, int age, float score);
    void show();
public:
    static int m_total;  // 声明了一个静态成员变量 m_total，用来统计学生的人数
private:
    char *m_name;
    int m_age;
    float m_score;
};
```

静态成员变量必须初始化
static 成员变量必须在类声明的外部初始化，具体形式为：
```c++
type class::name = value;
type 是变量的类型，class 是类名，name 是变量名，value 是初始值。将上面的 m_total 初始化：
int Student::m_total = 0;
```
是初始化时才为此静态成员变量分配内存（不是类声明时，也不是创建对象时），static 成员变量不占用对象的内存，而是在所有对象之外【在内存分区中的全局数据区】开辟内存，没有初始化的静态成员变量不能被使用。
因此，静态成员变量的内存是到程序结束时才释放，跟对象的创建和销毁没有任何关系（跟全局变量一个道理）

静态成员函数只能访问静态成员变量，静态成员函数与普通成员函数的根本区别在于：
普通成员函数能访问类中的任意成员，静态成员函数只能访问静态成员变量。

静态成员函数属于类，不属于任意一个对象，所以不需要 this 指针，因此无法访问普通成员变量，只能访问静态成员变量
下面例子，该例通过静态成员函数来获得学生的总人数和总成绩：
```cpp
#include <iostream>
using namespace std;

class Student{
public:
    Student(char *name, int age, float score);
    void show();
public:  // 1. 声明静态成员函数
    static int getTotal();
    static float getPoints();
private:
    static int m_total;  // 总人数
    static float m_points;  // 总成绩
private:
    char *m_name;
    int m_age;
    float m_score;
};

int Student::m_total = 0;
float Student::m_points = 0.0;

Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){//每次有参构造，对应的静态成员变量就加一
    m_total++;
    m_points += score;
}
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
// 2. 定义静态成员函数
int Student::getTotal(){
    return m_total;
}
float Student::getPoints(){
    return m_points;
}

int main(){
    (new Student("小明", 15, 90.6)) -> show();
    (new Student("李磊", 16, 80.5)) -> show();
    (new Student("张华", 16, 99.0)) -> show();
    (new Student("王康", 14, 60.8)) -> show();

    int total = Student::getTotal(); // 3. 使用静态成员函数读取静态成员变量
    float points = Student::getPoints();
    cout<<"当前共有"<<total<<"名学生，总成绩是"<<points<<"，平均分是"<<points/total<<endl;

    return 0;
}
运行结果：
复制代码小明的年龄是15，成绩是90.6  
李磊的年龄是16，成绩是80.5  
张华的年龄是16，成绩是99  
王康的年龄是14，成绩是60.8  
当前共有4名学生，总成绩是330.9，平均分是82.725
此例中，总人数 m_total 和总成绩 m_points 由各个对象累加得到，必须声明为 static 才能共享；getTotal()、getPoints() 分别用来获取总人数和总成绩，为了访问 static 成员变量，我们将这两个函数也声明为 static
```

**类中的const 成员变量和成员函数**
如果不希望成员变量和成员函数被修改，可以使用const关键字加以限定
const 成员变量的用法和普通 const 变量的用法相似，只需要在声明时加上 const 关键字。初始化 const 成员变量只有一种方法，就是通过构造函数的初始化列表
const 成员函数（常成员函数）
const 成员函数可以使用类中的所有成员变量，但是不能修改它们的值，这种措施主要还是为了保护数据而设置的。const 成员函数也称为常成员函数。
权限可以被缩小但不能被放大，普通对象可以调用普通成员函数，也可以调用const成员函数，只是在调用const成员函数时，无法进行’写’操作。而const对象一定不能进行写操作，所以不能调用普通成员函数。
我们通常将 get 函数设置为常成员函数。读取成员变量的函数的名字通常以get开头，后跟成员变量的名字，所以通常将它们称为 get 函数。
常成员函数需要在声明和定义的时候在函数头部的结尾（位置需要重点注意）加上 const 关键字，请看下面的例子：
```cpp
class Student{
public:
    Student(char *name, int age, float score);
    void show();
    // 声明 常成员函数
    char *getname() const;
    int getage() const;
    float getscore() const;
private:
    char *m_name;
    int m_age;
    float m_score;
};

Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){ }
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
//定义常成员函数
char * Student::getname() const{
    return m_name;
}
int Student::getage() const{
    return m_age;
}
float Student::getscore() const{
    return m_score;
}

需要注意：必须在成员函数的声明和定义处同时加上 const 关键字
区分一下 const 的位置：

函数开头的 const 用来修饰函数的返回值，表示返回值是 const 类型，也就是不能被修改，例如const char * getname()
函数头部的结尾加上 const 表示常成员函数，这种函数只能读取成员变量的值，而不能修改成员变量的值，例如char * getname() const

const 对象（常对象）
const 也可以用来修饰对象，称为常对象。一旦将对象定义为常对象之后，就只能调用类的 const 成员（包括 const 成员变量和 const 成员函数）了
const Student stu("小明", 15, 90.6);
const Student *pstu = new Student("李磊", 16, 80.5);
```

const使用细则：
https://juejin.cn/post/7023663671775592485

https://www.cnblogs.com/zixuanli/p/16909892.html


**C++类的友元**

友元函数

一个类 A 主动把一个外部函数 Fun（全局函数或另一个类 B 的成员函数）用关键字 friend 标记为友元函数，就表示主动允许外部函数 Fun 读取类 A 的 private 成员了
从本质上看，就是把对象指针传进友元函数，才能通过友元函数去读取类的 private 成员

1. 全局范围内的友元函数
目标：
声明一个 Student 类和一个全局函数 show
实现：全局函数 show 能读取 Student 类里的 private 成员变量 m_name、m_age、m_score
```c++
#include <iostream>
using namespace std;

class Student{
public:
    Student(char *name, int age, float score);
public:
    friend void show(Student *pstu);  // 1. 将show() 声明为友元函数
private:
    char *m_name;
    int m_age;
    float m_score;
};

Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){ }

// 2. 全局函数 show，形参为对象指针，通过对象指针读取 Student 类成员
void show(Student *pstu){
    cout<<pstu->m_name<<"的年龄是 "<<pstu->m_age<<"，成绩是 "<<pstu->m_score<<endl;
}

int main(){
    Student stu("小明", 15, 90.6);
    show(&stu);  // 3. 把对象指针 &stu 传进去调用友元函数
    Student *pstu = new Student("李磊", 16, 80.5);
    show(pstu);  // 3. 把对象指针 pstu 传进去调用友元函数

    return 0;
}
```
友元函数是不同于类的成员函数，在友元函数中不能直接访问类的成员，必须要借助对象，所以以上例子把 &stu 和 pstu 对象指针传进去，友元函数 show() 设置形参也为对象指针，通过对象指针才能去访问 Student 类里的成员变量和成员函数

2. 其他类的成员函数声明为友元函数
目标：

声明两个类：Student 类和 Address 类
实现：Student 类的成员函数 show 能读取 Address 类的 private 成员变量 m_province、m_city、m_district
```c++
#include <iostream>
using namespace std;

class Address;  // 提前声明 Address 类（先指定这个类是存在的）

// 声明 Student 类
class Student{
public:
    Student(char *name, int age, float score);
public:
    void show(Address *addr); // 1. Student 类写一个友元函数 show()，形参是 Address 类的对象指针
private:
    char *m_name;
    int m_age;
    float m_score;
};

// 声明 Address 类（先指定这个类包含这些成员）
class Address{
private:
    char *m_province;  // 省份
    char *m_city;  // 城市
    char *m_district;  // 区（市区）
public:
    Address(char *province, char *city, char *district);
    // 2. 将 Student 类中的成员函数 show() 声明为友元函数
    friend void Student::show(Address *addr);
};

// 定义 Student 类的构造函数
Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){ }
// 3. 定义 Student 类的 show 成员函数，接收形参 *addr 对象指针，可以读取 Address 类的成员变量
void Student::show(Address *addr){
    cout<<m_name<<"的年龄是 "<<m_age<<"，成绩是 "<<m_score<<endl;
    cout<<"家庭住址："<<addr->m_province<<"省"<<addr->m_city<<"市"<<addr->m_district<<"区"<<endl;
}

// 定义 Address 类的构造函数
Address::Address(char *province, char *city, char *district){
    m_province = province;
    m_city = city;
    m_district = district;
}

int main(){
    Student stu("小明", 16, 95.5f);
    Address addr("陕西", "西安", "雁塔");
    stu.show(&addr); // 4. 把对象指针 &addr 传入 stu 对象的成员函数 show 里面
   
    Student *pstu = new Student("李磊", 16, 80.5);
    Address *paddr = new Address("河北", "衡水", "桃城");
    pstu -> show(paddr); // 4. 把对象指针 paddr 传入 pstu 指向的对象的成员函数 show 里面

    return 0;
}

运行结果：
复制代码小明的年龄是 16，成绩是 95.5  
家庭住址：陕西省西安市雁塔区  
李磊的年龄是 16，成绩是 80.5  
家庭住址：河北省衡水市桃城区
```
3. 友元类：
一个类 A 主动把另一个类 B 用关键字 friend 标记为友元类，就表示主动允许读取类 A 的 private 成员了
除非有必要，一般不建议把整个类声明为友元类，而只将某些成员函数声明为友元函数，这样更安全一些

使用上只需要修改 Address 类即可，其余代码相同
```c++
// 声明 Address 类
class Address{
public:
    Address(char *province, char *city, char *district);
public:
    // 将Student类声明为Address类的友元类
    friend class Student;
private:
    char *m_province;  //省份
    char *m_city;  //城市
    char *m_district;  //区（市区）
};
```
关于友元，有两点需要说明：
友元的关系是单向的而不是双向的。如果声明了类 B 是类 A 的友元类，不等于类 A 是类 B 的友元类，类 A 中的成员函数不能访问类 B 中的 private 成员。
友元的关系不能传递。如果类 B 是类 A 的友元类，类 C 是类 B 的友元类，不等于类 C 是类 A 的友元类。


class 和 struct 的区别:
C++中的 struct 和 class 基本是通用的，唯有几个细节不同：

使用 class 时，类中的成员默认都是 private 属性的；而使用 struct 时，结构体中的成员默认都是 public 属性的
class 继承默认是 private 继承，而 struct 继承默认是 public 继承
class 可以使用模板，而 struct 不能

