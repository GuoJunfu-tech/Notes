# ROSCPP 标准格式
星期一, 25. 一月 2021 12:01下午 

郭俊甫

+ [原文](http://wiki.ros.org/CppStyleGuide)
_________

##几种命名方案：
1. **CamelCased**

2. **camelCased**

3. **under_scored**

4. **ALL_CAPITALS**

###具体情况：

* pakages 使用 **under_scored**

* topics and service names are **under_scored**

* 文件使用 **under_scored**
应该据有一定的描述性

如果文件主要实现一个类，请以该类命名。 例如，ActionServer类将位于文件action_server.h中。 

* Libraries使用**under_scored**
e.g

>libmy_great_thing ## lib之后不加"_ "

* 函数使用**camelCased**

函数要代表其作用，类一般使用名词

* 变量使用**under_scored**

较长的变量名字没有问题，因此要尽可能说清函数的作用。

迭代用的变量可用*i*, *j*等。

STL迭代器变量应指示要迭代的内容或者指向的类型。
```C++
std::list<int> pid_list;
std::list<int>::iterator pid_it;

std::list<int> pid_list;
std::list<int>::iterator int_it;
```
* 常量均**ALL_CAPITALS**

* 类的成员使用**under_scored_**

* 全局变量使用**g_under_scored**

* Namespace names are **under_scored**

## 格式

在VScode中使用`ctrl+shift+I`自动对齐即可。

## #ifndef guards

**所有标头**必须通过`#ifndef`防护措施保护免受多重包含.
要放在文件最前（license statement）之后。

## Documentation
> [doxygen](https://www.doxygen.nl/index.html)
> 
>[使用方法](https://www.cnblogs.com/rongpmcu/p/7662765.html)

## 输出
尽量使用[`rosconsole`](http://wiki.ros.org/rosconsole)输出，避免`cout`与`printf`。

## 输出参数
最好使用指针的形式，避免使用引用。
> int exampleMethod(FooThing input, BarThing* output);

## 命名空间

不要使用**using namespace**!!

可以使用：
> using std::vector

## 继承
继承是定义和实现公共接口的适当方法。 基类定义了接口，子类实现了该接口。

继承还可以用于提供从基类到子类的通用代码。 不鼓励使用继承。 在大多数情况下，“子类”可以包含“基类”的实例，并以较少的混淆可能性实现相同的结果。 

在子类中覆盖虚拟方法时，请始终将其声明为虚拟方法，以使读者知道发生了什么。 

##多重继承 

**尽量别**

## Exceptions

相较于`return(-1)`，多使用`Exceptions`。

始终在每个函数/方法上记录您的程序包可以抛出哪些异常。 不要抛出析构函数的异常。 不要从未直接调用的回调中引发异常。 如果您在软件包中选择使用错误代码代替异常，则仅使用错误代码。 始终如一。 

## 枚举
```C++
namespace Choices
{
  enum Choice
  {
  	Choice1,
   	Choice2,
   	Choice3
  };
}
typedef Choices::Choice Choice;
```
对于C++：
```C++
enum class Choise
{
    Choice1,
    Choice2,
    Choice3
};
Choise c = Choise::Choice1;
```

## 全局
**尽量别用全局变量和全局函数。**小型的帮手函数除外。

大多数变量和函数应在类内部声明。 其余部分应在名称空间中声明。 

例外：a file may contain a `main()` function and a handful of small helper functions that are global. But keep in mind that one day those helper function may become useful to someone else. 

## 尽量别用静态类变量

## exit()
Only call `exit()` at a well-defined exit point for the application.

Never call `exit()` in a library. 

##Testing
见[这里](http://wiki.ros.org/gtest)

## 迁移
* Don't use uint as a type. Instead use unsigned int.

* Call isnan() from within the std namespace, i.e.: `std::isnan()`
