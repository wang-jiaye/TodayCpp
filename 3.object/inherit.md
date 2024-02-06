## 11.类和对象
继承：
可以理解为一个类从另一个类获取成员变量和成员函数的过程。例如类 B 继承于类 A，那么 B 就拥有 A 的成员变量和成员函数

派生 和继承是一个概念，只是站的角度不同。
继承是儿子接收父亲的产业，派生是父亲把产业传承给儿子
被继承的类称为父类或基类
继承出来的类称为子类或派生类

派生类除了拥有基类的成员，还可以定义自己的新成员，以增强类的功能

两种典型的使用继承的场景：

1. 当你需要创建多个类，它们拥有很多相似的成员变量或成员函数时，可以使用继承。可以将这些类的共同成员提取出来，定义为基类，然后从基类继承，既可以节省代码，也方便后续修改相似成员
2. 当你创建的新类与现有的类相似，只是多出若干成员变量或成员函数时，可以使用继承，这样不但会减少代码量，而且新类会拥有基类的所有功能

例子：
基类 People 拥有共有的成员变量 m_name、m_name 和成员函数 setname()、setage()、getname()、getage()
派生类 Student 拥有继承与基类的成员，还有自己的成员变量 score 和成员函数 setscore()、getscore()
```cpp
#include<iostream>
using namespace std;

// 基类 People
class People{
public:
    void setname(char *name);
    void setage(int age);
    char *getname();
    int getage();
private:
    char *m_name;
    int m_age;
};
void People::setname(char *name){ m_name = name; }
void People::setage(int age){ m_age = age; }
char* People::getname(){ return m_name; }
int People::getage(){ return m_age;}

// 派生类 Student
class Student: public People{ // ☆ 继承于基类 People，拥有其成员变量和成员函数
public:
    void setscore(float score);
    float getscore();
private:
    float m_score;
};
void Student::setscore(float score){ m_score = score; }
float Student::getscore(){ return m_score; }

int main(){
    Student stu;
    stu.setname("小明");
    stu.setage(16);
    stu.setscore(95.5f);
    cout<<stu.getname()<<"的年龄是 "<<stu.getage()<<"，成绩是 "<<stu.getscore()<<endl;

    return 0;
}

由此总结出继承的一般语法为：
class 派生类名:［继承方式］ 基类名{
    派生类新增加的成员
};
```
继承方式包括 public（公有的）、private（私有的）和 protected（受保护的），此项是可选的，如果不写，那么默认为 private
|  继承方式 | public 成员  | protected 成员 | private 成员| 
|  ----| ----  |  ----  | ----  |
| public 继承| 在派生类里变成 public | 在派生类里变成 protected | 在派生类里不可见|
| protected 继承  | 在派生类里变成 protected | 在派生类里变成 protected	| 在派生类里不可见|
| private 继承  | 在派生类里变成 private | 在派生类里变成 private| 在派生类里不可见|
由于 private 和 protected 继承方式会改变基类成员在派生类中的访问权限，导致继承关系复杂，所以实际开发中我们一般使用 public继承方式

**派生类成员覆盖**

- 如果派生类中的成员（包括成员变量和成员函数）和基类中的成员重名，那么就会覆盖从基类继承过来的成员。在派生类中使用该成员（包括在定义派生类时使用，也包括通过派生类对象访问该成员）时，实际上使用的是派生类新增的成员，而不是从基类继承来的
```cpp
#include<iostream>
using namespace std;

// 基类 People
class People{
public:
    void show(); // 1. 声明基类的成员函数 show
protected:
    char *m_name;
    int m_age;
};
// 2. 定义基类的成员函数 show
void People::show(){
    cout<<"嗨，大家好，我叫"<<m_name<<"，今年"<<m_age<<"岁"<<endl;
}

// 派生类 Student
class Student: public People{
public:
    Student(char *name, int age, float score);
public:
    void show();  // 3. 派生类覆盖基类的成员函数 show
private:
    float m_score;
};
Student::Student(char *name, int age, float score){
    m_name = name;
    m_age = age;
    m_score = score;
}
// 4. 定义派生类成员函数 show
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}

int main(){
    Student stu("小明", 16, 90.5);
    // 5. 使用的是派生类新增的成员函数，而不是从基类继承的
    stu.show();
    // 5. 使用的是从基类继承来的成员函数
    stu.People::show();

    return 0;
}

运行结果：
小明的年龄是16，成绩是90.5  
嗨，大家好，我叫小明，今年16岁
```

**基类成员函数和派生类成员函数不构成重载**

如果派生类有同名函数，那么就会覆盖基类中的所有同名函数，不管它们的参数是否一样
```cpp
#include<iostream>
using namespace std;

// 基类 Base
class Base{
public:
    void func();
    void func(int);
};
void Base::func(){ cout<<"Base::func()"<<endl; }
void Base::func(int a){ cout<<"Base::func(int)"<<endl; }

// 派生类 Derived
class Derived: public Base{
public:
    void func(char *);
    void func(bool);
};
void Derived::func(char *str){ cout<<"Derived::func(char *)"<<endl; }
void Derived::func(bool is){ cout<<"Derived::func(bool)"<<endl; }

int main(){
    Derived d;
    d.func("c.biancheng.net");
    d.func(true);
    d.func();  // 编译器报错 compile error，派生类不存在这样的成员函数
    d.func(10);  // 编译器报错 compile error，派生类不存在这样的成员函数
    d.Base::func();
    d.Base::func(100);

    return 0;
}
```

类的构造函数不能被继承
- 在设计派生类时，对继承过来的成员变量的初始化工作也要由派生类的构造函数完成
如果要对基类的 private 类型的变量进行初始化，则要在派生类的构造函数中调用基类的构造函数
```cpp
#include<iostream>
using namespace std;

// 基类 People
class People{
protected:
    char *m_name;
    int m_age;
public:
    People(char*, int);
};
People::People(char *name, int age): m_name(name), m_age(age){}

// 派生类 Student
class Student: public People{
private:
    float m_score;
public:
    Student(char *name, int age, float score);
    void display();
};
// ☆ People(name, age) 就是调用基类的构造函数
Student::Student(char *name, int age, float score): People(name, age), m_score(score){ }
void Student::display(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<"。"<<endl;
}

int main(){
    Student stu("小明", 16, 90.5);
    stu.display();

    return 0;
}

People(name, age)就是调用基类的构造函数，并将 name 和 age 作为实参传递给它，m_score(score)是派生类的参数初始化表，它们之间以逗号,隔开
```

**基类构造函数调用规则**
- 通过派生类创建对象时必须要调用基类的构造函数，这是语法规定。定义派生类构造函数时最好指明（如上面例子）基类构造函数；如果不指明，系统自动调用基类的默认构造函数
```cpp
#include <iostream>
using namespace std;

// 基类 People
class People{
public:
    People(); // 1. 基类默认构造函数
    People(char *name, int age);
protected:
    char *m_name;
    int m_age;
};
People::People(): m_name("xxx"), m_age(0){ } // 1. 基类默认构造函数
People::People(char *name, int age): m_name(name), m_age(age){}

// 派生类 Student
class Student: public People{
public:
    Student(); // 2. 派生类默认构造函数
    Student(char*, int, float);
public:
    void display();
private:
    float m_score;
};
Student::Student(): m_score(0.0){ } // 2. 派生类默认构造函数
Student::Student(char *name, int age, float score): People(name, age), m_score(score){ }
void Student::display(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}

int main(){
    Student stu1;
    stu1.display();

    Student stu2("小明", 16, 90.5);
    stu2.display();

    return 0;
}

运行结果：
xxx的年龄是0，成绩是0
小明的年龄是16，成绩是90.5


创建对象 stu1 时，执行派生类的构造函数Student::Student()，它并没有指明要调用基类的哪一个构造函数，从运行结果可以很明显地看出来，系统默认调用了不带参数的构造函数，也就是People::People()
创建对象 stu2 时，执行派生类的构造函数Student::Student(char *name, int age, float score)，它指明了基类的构造函数，所以没有继续往上走
```

**基类和派生类的析构函数**
- 析构函数的执行顺序和构造函数的执行顺序也刚好相反：
创建派生类对象时，构造函数的执行顺序和继承顺序相同，即先执行基类构造函数，再执行派生类构造函数
销毁派生类对象时，析构函数的执行顺序和继承顺序相反，即先执行派生类析构函数，再执行基类析构函数

```cpp
#include <iostream>
using namespace std;

class A{
public:
    A(){cout<<"A constructor"<<endl;}
    ~A(){cout<<"A destructor"<<endl;}
};

class B: public A{
public:
    B(){cout<<"B constructor"<<endl;}
    ~B(){cout<<"B destructor"<<endl;}
};

class C: public B{
public:
    C(){cout<<"C constructor"<<endl;}
    ~C(){cout<<"C destructor"<<endl;}
};

int main(){
    C test;
    return 0;
}

运行结果：
A constructor  
B constructor  
C constructor  
C destructor  
B destructor  
A destructor
```

**多重继承**

一个派生类可以继承自多个基类，称为多重继承

```cpp
#include <iostream>
using namespace std;

// 基类 BaseA
class BaseA{
public:
    BaseA(int a, int b);
    ~BaseA();
protected:
    int m_a;
    int m_b;
};
BaseA::BaseA(int a, int b): m_a(a), m_b(b){
    cout<<"BaseA constructor"<<endl;
}
BaseA::~BaseA(){
    cout<<"BaseA destructor"<<endl;
}

// 基类 BaseB
class BaseB{
public:
    BaseB(int c, int d);
    ~BaseB();
protected:
    int m_c;
    int m_d;
};
BaseB::BaseB(int c, int d): m_c(c), m_d(d){
    cout<<"BaseB constructor"<<endl;
}
BaseB::~BaseB(){
    cout<<"BaseB destructor"<<endl;
}

// 派生类 Derived（独有变量 e），同时继承自 BaseA（变量 a b） 和 BaseB（变量 c d）
class Derived: public BaseA, public BaseB{
public:
    Derived(int a, int b, int c, int d, int e);
    ~Derived();
public:
    void show();
private:
    int m_e;
};
Derived::Derived(int a, int b, int c, int d, int e): BaseA(a, b), BaseB(c, d), m_e(e){
    cout<<"Derived constructor"<<endl;
}
Derived::~Derived(){
    cout<<"Derived destructor"<<endl;
}
void Derived::show(){
    cout<<m_a<<", "<<m_b<<", "<<m_c<<", "<<m_d<<", "<<m_e<<endl;
}

int main(){
    Derived obj(1, 2, 3, 4, 5);
    obj.show();
    return 0;
}
运行结果：
BaseA constructor  
BaseB constructor  
Derived constructor  
1, 2, 3, 4, 5  
Derived destructor  
BaseB destructor  
BaseA destructor
```

如果 BaseA 和 BaseB 类都有 show 函数，那么继承自他们的派生类 Derived 使用 show 函数就得用域运算符::来指定是使用哪个基类的 show 函数

**虚拟继承和虚基类**
- 多继承例如菱形继承，很容易产生命名冲突：
如果B与C继承于A，而D多继承于B和C。
假如类 A 有一个成员变量 a，那么在类 D 中直接访问 a 就会产生歧义，编译器不知道它究竟来自 A -->B-->D 这条路径，还是来自 A-->C-->D 这条路径
为了解决多继承时的命名冲突和冗余数据问题，C++ 提出了虚继承，使得在派生类中只保留一份间接基类的成员
在继承方式前面加上 virtual 关键字就是虚继承，请看下面的例子：
```cpp
// 间接基类A
class A{
protected:
    int m_a;
};

// 直接基类B
class B: virtual public A{  // 虚继承
protected:
    int m_b;
};

// 直接基类C
class C: virtual public A{  // 虚继承
protected:
    int m_c;
};

// 派生类 D
class D: public B, public C{
public:
    void seta(int a){ m_a = a; }  // 正确
    void setb(int b){ m_b = b; }  // 正确
    void setc(int c){ m_c = c; }  // 正确
    void setd(int d){ m_d = d; }  // 正确
private:
    int m_d;
};

int main(){
    D d;
    return 0;
}
```
这样在派生类 D 中就只保留了一份成员变量 m_a，直接访问就不会再有歧义了
虚继承的目的是让某个类做出声明，承诺愿意共享它的基类。其中，这个被共享的基类就称为虚基类（Virtual Base Class），本例中的 A 就是一个虚基类。在这种机制下，不论虚基类在继承体系中出现了多少次，在派生类中都只包含一份虚基类的成员
C++标准库中的 iostream 类就是一个虚继承的实际应用案例。iostream 从 istream 和 ostream 直接继承而来，而 istream 和 ostream 又都继承自一个共同的名为 base_ios 的类，是典型的菱形继承。此时 istream 和 ostream 必须采用虚继承，否则将导致 iostream 类中保留两份 base_ios 类的成员。

**向上转型**

将派生类对象赋值给基类对象、将派生类指针赋值给基类指针、将派生类引用赋值给基类引用，这在 C++ 中称为向上转型
- 1.将派生类对象赋值给基类对象
```cpp
#include <iostream>
using namespace std;

// 基类
class A{
public:
    A(int a);
public:
    void display();
public:
    int m_a;
};
A::A(int a): m_a(a){ }
void A::display(){
    cout<<"Class A: m_a="<<m_a<<endl;
}

// 派生类
class B: public A{
public:
    B(int a, int b);
public:
    void display();
public:
    int m_b;
};
B::B(int a, int b): A(a), m_b(b){ }
void B::display(){
    cout<<"Class B: m_a="<<m_a<<", m_b="<<m_b<<endl;
}

int main(){
    A a(10);
    B b(66, 99);
    // 赋值前
    a.display();
    b.display();
    cout<<"--------------"<<endl;
    // 赋值后
    a = b;
    a.display();
    b.display();

    return 0;
}

运行结果：
Class A: m_a=10  
Class B: m_a=66, m_b=99  
----------------------------  
Class A: m_a=66 // 这里变成了 66
Class B: m_a=66, m_b=99
```
本例中 A 是基类， B 是派生类，a、b 分别是它们的对象，由于派生类 B 包含了从基类 A 继承来的成员，因此可以将派生类对象 b 赋值给基类对象 a。通过运行结果也可以发现，赋值后 a 所包含的成员变量的值已经发生了变化
赋值的本质是将现有的数据写入已分配好的内存中，对象的内存只包含了成员变量，所以对象之间的赋值是成员变量的赋值，成员函数不存在赋值问题。运行结果也有力地证明了这一点，虽然有a=b这样的赋值过程，但是 a.display() 始终调用的都是 A 类的 display() 函数。换句话说，对象之间的赋值不会影响成员函数，也不会影响 this 指针
将派生类对象赋值给基类对象时，会舍弃派生类新增的成员，也就是大材小用
！[图片]()

- 2.将派生类指针赋值给基类指针
  
除了可以将派生类对象赋值给基类对象（对象变量之间的赋值），还可以将派生类指针赋值给基类指针（对象指针之间的赋值）。我们先来看一个多继承的例子，继承关系为：
```cpp
#include <iostream>
using namespace std;

// 基类 A
class A{
public:
    A(int a);
public:
    void display();
protected:
    int m_a;
};
A::A(int a): m_a(a){ }
void A::display(){
    cout<<"Class A: m_a="<<m_a<<endl;
}

// 中间派生类 B
class B: public A{
public:
    B(int a, int b);
public:
    void display();
protected:
    int m_b;
};
B::B(int a, int b): A(a), m_b(b){ }
void B::display(){
    cout<<"Class B: m_a="<<m_a<<", m_b="<<m_b<<endl;
}

// 基类 C
class C{
public:
    C(int c);
public:
    void display();
protected:
    int m_c;
};
C::C(int c): m_c(c){ }
void C::display(){
    cout<<"Class C: m_c="<<m_c<<endl;
}

// 最终派生类 D
class D: public B, public C{
public:
    D(int a, int b, int c, int d);
public:
    void display();
private:
    int m_d;
};
D::D(int a, int b, int c, int d): B(a, b), C(c), m_d(d){ }
void D::display(){
    cout<<"Class D: m_a="<<m_a<<", m_b="<<m_b<<", m_c="<<m_c<<", m_d="<<m_d<<endl;
}

int main(){
    A *pa = new A(1);
    B *pb = new B(2, 20);
    C *pc = new C(3);
    D *pd = new D(4, 40, 400, 4000);

    pa = pd;
    pa -> display();

    pb = pd;
    pb -> display();

    pc = pd;
    pc -> display();

    cout<<"-----------------------"<<endl;
    cout<<"pa="<<pa<<endl;
    cout<<"pb="<<pb<<endl;
    cout<<"pc="<<pc<<endl;
    cout<<"pd="<<pd<<endl;

    return 0;
}

运行结果：
Class A: m_a=4  
Class B: m_a=4, m_b=40  
Class C: m_c=400  
-----------------------  
pa=0x9b17f8  
pb=0x9b17f8  
pc=0x9b1800  
pd=0x9b17f8
```
本例中定义了多个对象指针，并尝试将派生类指针赋值给基类指针。与对象变量之间的赋值不同的是，对象指针之间的赋值并没有拷贝对象的成员，也没有修改对象本身的数据，仅仅是改变了指针的指向
1) 通过基类指针访问派生类的成员
请读者先关注第 68 行代码，我们将派生类指针 pd 赋值给了基类指针 pa，从运行结果可以看出，调用 display() 函数时虽然使用了派生类的成员变量，但是 display() 函数本身却是基类的。也就是说，将派生类指针赋值给基类指针时，通过基类指针只能使用派生类的成员变量，但不能使用派生类的成员函数，这看起来有点不伦不类，究竟是为什么呢？

第 71、74 行代码也是类似的情况
pa 本来是基类 A 的指针，现在指向了派生类 D 的对象，这使得隐式指针 this 发生了变化，也指向了 D 类的对象，所以最终在 display() 内部使用的是 D 类对象的成员变量，相信这一点不难理解
编译器虽然通过指针的指向来访问成员变量，但是却不通过指针的指向来访问成员函数：编译器通过指针的类型来访问成员函数。对于 pa，它的类型是 A，不管它指向哪个对象，使用的都是 A 类的成员函数
概括起来说就是：编译器通过指针来访问成员变量，指针指向哪个对象就使用哪个对象的数据；编译器通过指针的类型来访问成员函数，指针属于哪个类的类型就使用哪个类的函数

2) 赋值后值不一致的情况
本例中我们将最终派生类的指针 pd 分别赋值给了基类指针 pa、pb、pc，按理说它们的值应该相等，都指向同一块内存，但是运行结果却有力地反驳了这种推论，只有 pa、pb、pd 三个指针的值相等，pc 的值比它们都大。也就是说，执行pc = pd语句后，pc 和 pd 的值并不相等
这非常出乎我们的意料，按照我们通常的理解，赋值就是将一个变量的值交给另外一个变量，不会出现不相等的情况