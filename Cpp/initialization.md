# Initialization

## 默认初始化 (default initialized)

如果定义变量是未指定初始值，则变量被默认初始化(default initialized)。

* 内置类型:
    * 定义于任何函数体之外的变量被初始化为0
    * 定义于函数体之内的变量不被初始化(uninitialized), 该变量的值是未定义的
* 类类型:
    * 由是否由默认构造函数，以及默认构造函数的行为决定

## 值初始化 (value-initialized)

在标准库容器中, 如果初始化时只提供元素数量不提供初始值, 则会创建值初始化的元素初值, 并把它赋给容器中的元素。

这个初值由容器中对象的类型决定
* 内置类型
    * 初始值为0
* 类类型
    * 由类默认初始化。

## 列表初始化 (list initialization)

无论是初始化对象还是某些时候为对象赋新值，都可以使用由花括号括起来的初始值
```cpp
// 以下4条语句等价
int units_sold = 0;
int units_sold = {0};
int units_sold(0);
int units_sold{0};
```
当用于内置类型变量时，如果使用列表初始化且初始值存在丢失信息的风险，则编译器将报错
```cpp
long double pi = 3.1415926536;
int a{pi}, b = {pi}; // 错误：转换未执行，因为存在丢失信息的风险
int c(ld), d = pi;   // 正确：转换执行，且确实丢失了部分值
```

## initalizer_list<T>
C++ primer 6.2.6节