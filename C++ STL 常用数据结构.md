# vector

## 初始化

[(4 条消息) vector 的几种初始化及赋值方式\_e891377 的专栏 - CSDN 博客](https://blog.csdn.net/e891377/article/details/108929556)

### 有初始值初始化

```c++
//初始化size,但每个元素值为默认值
vector<int> abc(10);    //初始化了10个默认值为0的元素
//初始化size,并且设置初始值
vector<int> cde(10，1);    //初始化了10个值为1的元素
vector<int> c{1,2,3,4,5};
```

### 是有迭代器初始化

```c++
vector<int> pre
ector<int> left_pre(pre.begin() + 1, pre.begin() + mid + 1);   //包括根节点在内的左子树，也可能不是，这个构造函数输入的可能是左开右闭区间[ )  需要测试； 经过测试确实是这样的。
vector<int> right_pre(pre.begin() + mid + 1, pre.end());  // 发生了拷贝3构造
```

#### 不合法情况

```c++
	vector<int> c{1,2,3,4,5};
    vector<int> f(c.begin()+1, c.begin()+1); //合法
	printf("f.size()=%d\n",f.size());   //输出结果为0

	vector<int> g(c.begin() + 2, c.begin() + 1);  //不合法运行时错误
	printf("f.size()=%d\n", g.size());


	vector<int> h(c.begin() + 6, c.begin() + 6);  //不合法
	printf("h.size()=%d\n", h.size());

	vector<int> o(c.begin() + 5, c.begin() + 5); //等价于c.end()  合法 成功创建
	printf("o.size()=%d\n", o.size());
```

### 一些测试程序

代码

```c++
// code_inerview.cpp: 定义应用程序的入口点。
//

#include "code_inerview.h"
#include<regex>
using namespace std;


int main()
{
	int array[5] = {1,2,3,4,5};
	
	vector<int> a(array,array+5); //判断是不是发生了copy
	vector<int> b = a;
	vector<int> c{1,2,3,4,5};
	int d{ 1000 }; //初始化的一种方式
	vector<int> e(c.begin(), c.begin()+2);//发生了拷贝构造 根据左开右闭原则 其实只有2个元素在里面  即 c[0] c[1]
	printf("d = %d\n", d);

	for (int i = 0; i < c.size(); i++)
	{
		printf("c[%d] = %d\n", i, c[i]++);
	}


	for (int i = 0; i < e.size(); i++)
	{
		printf("e[%d] = %d\n", i, e[i]++);
	}

	for (int i=0; i<a.size(); i++)
	{
		printf("a[%d] = %d\n",i,a[i]++);
	}

	for (int i = 0; i < a.size(); i++)
	{
		printf("array[%d] = %d\n", i, array[i]++);
	}

	for (int i = 0; i < a.size(); i++)
	{
		printf("b[%d] = %d\n", i, b[i]++);
	}

	for (int i = 0; i < a.size(); i++)
	{
		printf("a[%d] = %d\n", i, a[i]++);
	}

	return 0;
}
```

输出结果

```
d = 1000
c[0] = 1
c[1] = 2
c[2] = 3
c[3] = 4
c[4] = 5
e[0] = 1
e[1] = 2
a[0] = 1
a[1] = 2
a[2] = 3
a[3] = 4
a[4] = 5
array[0] = 1
array[1] = 2
array[2] = 3
array[3] = 4
array[4] = 5
b[0] = 1
b[1] = 2
b[2] = 3
b[3] = 4
b[4] = 5
a[0] = 2
a[1] = 3
a[2] = 4
a[3] = 5
a[4] = 6
```

## 通过数组地址初始化

```c++
int a[5] = {1,2,3,4,5};
//通过数组a的地址初始化，注意地址是从0到5（左闭右开区间）
vector<int> b(a, a+5);   //发生了拷贝构造 
```

## 关于vector< int >:: iterator it = vi.begin()

vi.begin()是vector的第一个元素地址

vi.end()是vector的最后一个元素的下一个元素的地址 这是遵循左开右闭的原则

## tips

vi[i] 与 vi.begin()+i等价

