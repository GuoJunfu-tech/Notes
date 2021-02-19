# lambda 表达式

> 语法：
> `[arglist](args){function};`
> 
> 'arglist'从局部变量中捕获，其中一个变量可以设置为‘=’，引用则设置为‘&’，编译器自动去寻找
> `args`为输入的变量

e.g.
```
int sum = 35;
auto g = [&sum](int s) { return (s > sum) ? "s>sum" : "s<=sum"; };
int s = 100;
cout << g(s) << endl;
```
输出：`s>sum`

----------
# 其它算法

## 函数对象


* 函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
* 函数对象超出普通函数的概念，函数对象可以有自己的状态
* **函数对象可以作为参数传递**

```
#include <string>

//1、函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
class MyAdd
{
public :
	int operator()(int v1,int v2)
	{
		return v1 + v2;
	}
};

void test01()
{
	MyAdd myAdd;
	cout << myAdd(10, 10) << endl;
}

//2、函数对象可以有自己的状态
class MyPrint
{
public:
	MyPrint()
	{
		count = 0;
	}
	void operator()(string test)
	{
		cout << test << endl;
		count++; //统计使用次数
	}

	int count; //内部自己的状态
};
void test02()
{
	MyPrint myPrint;
	myPrint("hello world");
	myPrint("hello world");
	myPrint("hello world");
	cout << "myPrint调用次数为： " << myPrint.count << endl;
}

//3、函数对象可以作为参数传递
void doPrint(MyPrint &mp , string test)
{
	mp(test);
}

void test03()
{
	MyPrint myPrint;
	doPrint(myPrint, "Hello C++");
}

int main() {

	//test01();
	//test02();
	test03();

	system("pause");

	return 0;
}
```

## 常用算法

### for each

```
#include <algorithm>
#include <vector>

//普通函数
void print01(int val) 
{
	cout << val << " ";
}
//函数对象
class print02 
{
 public:
	void operator()(int val) 
	{
		cout << val << " ";
	}
};

//for_each算法基本用法
void test01() {

	vector<int> v;
	for (int i = 0; i < 10; i++) 
	{
		v.push_back(i);
	}

	//遍历算法
	for_each(v.begin(), v.end(), print01);
	cout << endl;

	for_each(v.begin(), v.end(), print02());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```
### transform

```
#include<vector>
#include<algorithm>

//常用遍历算法  搬运 transform

class TransForm
{
public:
	int operator()(int val)
	{
		return val;
	}

};

class MyPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int>v;
	for (int i = 0; i < 10; i++)
	{
		v.push_back(i);
	}

	vector<int>vTarget; //目标容器

	vTarget.resize(v.size()); // 目标容器需要提前开辟空间

	transform(v.begin(), v.end(), vTarget.begin(), TransForm());

	for_each(vTarget.begin(), vTarget.end(), MyPrint());
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

### merge

* 容器元素合并，并存储到另一容器中

* 合并中进行排序

**注意: 两个容器必须是有序的**

```
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10 ; i++) 
    {
		v1.push_back(i);
		v2.push_back(2*i);
	}

	vector<int> vtarget;
	//目标容器需要提前开辟空间
	vtarget.resize(v1.size() + v2.size());
	//合并  需要两个有序序列
	merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vtarget.begin());
	for_each(vtarget.begin(), vtarget.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```
> 输出：` 0 0 1 2 2 3 4 4 5 6 6 7 8 8 9 10 12 14 16 18 `

----------

# 算法与lambda结合

循环：
## for_each
```
std::vector<int> values{2, 0, 12, 3, 5, 0, 2, 7, 0, 8};
int sum02 = 0;
int min = 3;
for_each(values.begin(), values.end(),
            [=, &sum02](int v) {
               if (v >= min)
                  sum02 += v;
            });
```
得到向量中大于2的数值之和。

## accumulate
```
auto sum = std::accumulate(std::begin(values), std::end(values), 5,
                            [min](int s, int v) {
                               cout << s << " ";
                               if (v < min)
                                  return s;
                               return s + v;
                            });
```
得到向量中大于2的数值之和。

这里的`s`与`v`按照出现顺序分别代表`sum` 与 迭代器
