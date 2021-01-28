# C++ 类

[地址](https://www.bilibili.com/video/BV1et411b73Z?p=99)

属性 + 行为+访问权限

* 属性：自带的变量

* 行为：类中的函数

* 属性和行为统称为**成员**

* 对象：使用类的具体的对象

```
//圆周率
const double PI = 3.14;

//1、封装的意义
//将属性和行为作为一个整体，用来表现生活中的事物

//封装一个圆类，求圆的周长
//class代表设计一个类，后面跟着的是类名
class Circle
{
public:  //访问权限  公共的权限

	//属性
	int m_r;//半径

	//行为
	//获取到圆的周长
	double calculateZC()
	{
		//2 * pi  * r
		//获取圆的周长
		return  2 * PI * m_r;
	}
};

int main() {

	//通过圆类，创建圆的对象
	// c1就是一个具体的圆
	Circle c1;
	c1.m_r = 10; //给圆对象的半径 进行赋值操作

	//2 * pi * 10 = = 62.8
	cout << "圆的周长为： " << c1.calculateZC() << endl;

	system("pause");

	return 0;
}
```

* 实例化：通过一个类创建一个对象

## struct and class

* **struct** 默认权限是 **public**
  
* **class** 默认权限是 **private**

## 成员属性设置为私有

即，对`private`中的属性，可以设置`public`中的接口来进行读写。

## 构造函数和析构函数

> *不写的话编译器自动提供*

### 构造函数：创建时自动赋值

语法：`类名(){}`
无返回值，也不写`void`
只调用一次

### 析构函数：清理作用

语法：`~类名(){}`
无返回值，也不写`void`
只调用一次

### 普通构造和拷贝构造函数

e.g: 
```
class Person {
public:
	//无参（默认）构造函数
	Person() {
		cout << "无参构造函数!" << endl;
	}
	//有参构造函数
	Person(int a) {
		age = a;
		cout << "有参构造函数!" << endl;
	}
	//拷贝构造函数
	Person(const Person& p) {
		age = p.age;
		cout << "拷贝构造函数!" << endl;
	}
	//析构函数
	~Person() {
		cout << "析构函数!" << endl;
	}
public:
	int age;
};

//2、构造函数的调用
//调用无参构造函数
void test01() {
	Person p; //调用无参构造函数
}

//调用有参的构造函数
void test02() {

	//2.1  括号法，常用
	Person p1(10);
	//注意1：调用无参构造函数不能加括号，如果加了编译器认为这是一个函数声明
	//Person p2();

	//2.2 显式法
	Person p2 = Person(10); 
	Person p3 = Person(p2);
	//Person(10)单独写就是匿名对象  当前行结束之后，马上析构

	//2.3 隐式转换法
	Person p4 = 10; // Person p4 = Person(10); 
	Person p5 = p4; // Person p5 = Person(p4); 

	//注意2：不能利用 拷贝构造函数 初始化匿名对象 编译器认为是对象声明
	//Person p5(p4);
}

int main() {

	test01();
	//test02();

	system("pause");

	return 0;
}
```
> 拷贝构造函数:`Person(const Person& p)`
> 
> 调用无参构造函数不能加括号，如果加了编译器认为这是一个函数声明
> 
> `Person(10)`单独写就是匿名对象  当前行结束之后，马上析构--> 可以函数传参的时候用
> 
> 不能利用 拷贝构造函数 初始化匿名对象 编译器认为是对象声明

### 拷贝构造函数的调用时机

* 默认构造函数(无参，函数体为空)

* 默认析构函数(无参，函数体为空)

* 默认拷贝构造函数，对属性进行值拷贝

### 调用规则

* 如果用户定义有参构造函数，c++不在提供默认无参构造，但是会提供默认拷贝构造

* 如果用户定义拷贝构造函数，c++不会再提供其他构造函数

### 深拷贝与浅拷贝
