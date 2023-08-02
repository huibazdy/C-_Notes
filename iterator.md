> ***C++ 迭代器是什么？***

* 为什么要有迭代器？
* 本质上是指针吗？
* 底层实现是什么？



> ***C++ 迭代器类型***

1. **`iterator`**

    正向迭代器。

2. **`const_iterator`**

    常量正向迭代器。

3. **`reverse_iterator`**

    反向迭代器。

4. **`const_reverse_iterator`**

    常量反向迭代器。



> ***基本使用***

以`iterator`使用为例，用迭代器遍历非关联容器 vector

```c++
#include<iostream>
#include<vector>
using namespace std;

int main()
{
    //定义并初始化一个vector，内部元素为：0~4
    vector<int> v;
    for(int i = 0; i < 5; i++)
        v.push_back(i);
    
    //定义一个正向迭代器，用于遍历和访问 v
    vector<int>::iterator i;
    
    //利用迭代器遍历容器，并打印元素值
    for(i = v.begin(); i != v.end(); ++i)
        cout<<*i<<" ";
    cout<<endl;
    
    //定义一个反向迭代器，用于遍历和访问 v
    vector<int>::reverse_iterator j;
    
    //反向迭代器遍历容器，并打印元素值
    for(j = v.rbegin(); j != v.rend(); ++j)  
        cout<<*j<<" ";
    cout<<endl;
    
    return 0;
}
```



> ***begin( ) 与 end( )***

`begin()`和`end()`都是模板函数，返回的是容器的正向迭代器。前者返回的迭代器指向容器的首元素，后者返回的迭代器指向容器末位元素的下一个位置。



> ***rbegin( ) 与 rend( )***

`rbegin()`和`rend()`返回的是反向迭代器，前者返回的迭代器指向容器的最后一个元素，后者返回的迭代器指向容器首元素的前一个位置。



> ***cbegin( ) 与 cend( )***

C++ 有两种迭代器，分别是`iterator` 和`const_iterator`。它们的作用是遍历、访问容器内的元素。其中前者可以用来修改容器元素的值，而后者不可以。可以理解为指向常量的指针。

`cbegin()`和`cend()`是 **C++11** 新增的，返回的是 **const_iterator**，不能修改元素。



> ***迭代器辅助函数***

定义在头文件`<algorithm>`中，若要使用需先包含。三个模板函数分别是：

* `advance(p,n)`：迭代器p向后移动n个位置
* `distance(p,q)`：计算迭代器p与a的距离，即p多少次自增操作后与q相等，如果初始状态p已在q后，会陷入死循环
* `iter_swap(p,q)`：用于交换两个迭代器指向的值