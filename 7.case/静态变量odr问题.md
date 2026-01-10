## 问题：
在对应的参数系统中某个头文件中存在static定义的静态变量, 其在.h中被修改过。对应不同文件的接口A（在.h内）和接口B分别将其读取，发现读取到的值并不是自己修改后的值，请问这是为什么？如何解决？

### 原因：
这是因为在C++中，使用static关键字定义的变量具有内部链接（internal linkage），这意味着每个翻译单元（通常对应一个cpp文件）都会有自己独立的静态变量副本。

因此，当接口A和接口B分别修改这个静态变量时，它们实际上是在修改各自翻译单元中的不同副本，导致读取到的值并不是自己修改后的值。

在头文件中使用static定义变量，每个包含该头文件的源文件都会获得独立的变量实例
这些变量虽然同名，但在内存中是不同的存储位。违反了c++的“一定义规则”（ODR，One Definition Rule）。
### 解法：
使用extern声明
- .h
```c++
// config.h
#ifndef CONFIG_H
#define CONFIG_H

// 只做声明，不定义。 在源文件中定义一次，后续包含对应的只是声明，告诉编译器"这个变量在别处定义"
extern int config_value;

void set_config(int value);
int get_config(void);

#endif
```
- .cpp
```c++
// config.c
#include "config.h"

// 在源文件中定义一次，后续包含对应的只是声明，告诉编译器"这个变量在别处定义"
int config_value = 0;

void set_config(int value) {
    config_value = value;
}

int get_config(void) {
    return config_value;
}
```

| 类型   | 变量      | 是否共享  |
| -----------| ------------| ----- |
| 头文件中的static    | 每个引用头文件的cpp一个  | 不共享|
| extern声明    | 全局一个  | 共享|

记住：在多线程环境中，任何对共享数据的非原子读写都需要同步。extern声明的全局变量本身不是线程安全的，必须配合适当的同步机制（锁，原子变量）使用。
