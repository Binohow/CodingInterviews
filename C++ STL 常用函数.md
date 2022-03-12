[toc]

# find

find () 为在输入迭代器所定义的范围内查找单个对象的算法，它可以在前两个参数指定的范围内查找和第三个参数相等的第一个对象。

find 算法会返回一个指向被找到对象的迭代器，如果没有找到对象，会返回这个序列的结束迭代器。下面展示了如何使用 find ():

```c++
std::vector<int> numbers {5, 46, -5, -6, 23, 17, 5, 9, 6, 5};
int value {23};
auto iter = std::find(std::begin(numbers), std::end(numbers), value);
if (iter != std:: end (numbers))
    std::cout << value << " was found. \n";
```

# lambda

## 定义

一般是有auto推断lambda类型

```
auto less {[](int x, int y){return x<y;}}
```

## lambda引导[]不一定是空的，可以包含捕获字句

如果[]内为空，则lambda表达式体只能使用lambda表达式中的局部定义的实参和变量

# find_if

```c++
std::vector<int> numbers {5, 46, -5, -6, 23, 17, 5, 9, 6, 5};
int value {23};
auto iter = std::find(std::begin(numbers), std::end(numbers), value);
if (iter != std:: end (numbers))
    std::cout << value << " was found. \n"
```

# distance

我们知道，作用于同一容器的 2 个同类型迭代器可以有效指定一个区间范围。在此基础上，如果想获取该指定范围内包含元素的个数，就可以借助本节要讲的 distance() 函数。

distance () 函数用于计算两个迭代器表示的范围内包含元素的个数

```c++
#include <iostream>     // std::cout
#include <iterator>     // std::distance
#include <list>         // std::list
using namespace std;

int main() {
    //创建一个空 list 容器
    list<int> mylist;
    //向空 list 容器中添加元素 0~9
    for (int i = 0; i < 10; i++) {
        mylist.push_back(i);
    }
    //指定 2 个双向迭代器，用于执行某个区间
    list<int>::iterator first = mylist.begin();//指向元素 0
    list<int>::iterator last = mylist.end();//指向元素 9 之后的位置   mylist.end()尾部元素地址下一个地址
    //获取 [first,last) 范围内包含元素的个数
    cout << "distance() = " << distance(first, last);
    return 0;
}
```

程序执行结果为：

```
distance() = 10
```

