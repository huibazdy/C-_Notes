# 一、泛型





# 二、基于对象

C++本身就提供了很多我们经常用到的类，例如：string、vector等。

* 类对象有很多种初始化方法，如string
* 每个类都有一组操作函数，如`empty()`、`size()`等



有时候我们不得不自己设计符合自己需求的类，一般而言，类由两部分组成：

1. `public`：操作函数、运算符。二者称为成员函数，代表class的公开接口供class的用户使用。
2. `private`：实现细节。一般由与class相关的数据（只能被类成员函数或友元类访问），以及操作函数的实现（类内实现会自动被视为**inline**函数）细节组成。



【实现一个栈】

设计一个类首先以该从抽象开始。以栈这种数据结构为例，涉及到压入数据`push()`、取出数据`pop()`、压入前确认栈是否已满`full()`、取数据前确认是否为空`empty()`、查询栈内元素个数`size()`、查看栈顶元素`peek()`等操作。

```C++
class Stack
{
    public:
        bool push(const string&);
        bool pop(string &elem);
        bool empty();
        bool full();
        bool peek(string &elem);
        int size(){return _stack.size();}
    private:
        vector<string> _stack;  //在data member前加短横线
};
```





# 三、面向对象