# 一、泛型





# 二、基于对象

C++本身就提供了很多我们经常用到的类，例如：string、vector等。

* 类对象有很多种初始化方法，如string
* 每个类都有一组操作函数，如`empty()`、`size()`等



有时候我们不得不自己设计符合自己需求的类，一般而言，类由两部分组成：

1. `public`：操作函数、运算符。二者称为成员函数，代表class的公开接口供class的用户使用。
2. `private`：实现细节。一般由与class相关的数据（只能被类成员函数或友元类访问），以及操作函数的实现（类内实现会自动被视为**inline**函数）细节组成。



【**实现一个栈**】

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

栈的元素填充：

```C++
void fill_stack(Stack &stack,istream &is = cin)
{
    string str;
    while(is >> str && !stack.full())
        stack.push(str);
    cout<<"Read in"<<stack.size()<<"elements\n";
}
```

成员函数的实现：

```C++
inline bool Stack::empty()   //inline成员函数定义都应该放在Stack.h文件中
{
    return _stack.empty();
}

inline bool Stack::full()    
{
    return _stack.size() == _stack.max_size();
}

bool Stack::pop(string &elem)  //非inline成员函数定义应该放在程序代码文件Stack.cpp中
{
    if(empty())
        return false;
    elem = _stack.back();
    return true;
}

bool Stack::peek(string &elem)
{
    if(empty())
        return false;
    elem = _stack.back();
    return true;
}

bool Stack::push(const string &elem)
{
    if(full())
        return false;
    _stack.push_back(elem);
    return true;
}
```



> 到此为止，还未完成Stack类的完整定义，还需要加入构造与析构函数。



【**数列引入构造函数**】

每个数列很适合设计为类，每个数列需要记住自己的长度、元素个数、起始位置等。

例如将斐波那契数列抽象为`Fibonacci`类：

`Fibonacci fib1(7,3);  //七个元素，从位置3开始`

佩尔数列（a1=0，a2=1，...，an=2*an-1+an-2）抽象为`Pell`类：

`Pell pel(10);    //十个元素，默认从位置1开始`

斐波那契数列拷贝初始化;

`Fibonacci fib2(fib1);  //fib2是fib1的副本`



```C++
class Triangular  //Triangular数列类
{
    public:
        ...
    private:
        int _length;    //长度
        int _beginPos;  //起始位置
        int _next;      //下一个元素
};
```



我们需要考虑 如何初始化，因为编译器不会自动处理。因此我们需要编写用于初始化数据成员的***构造函数***。

构造函数需要遵循以下法则：

* 无返回值
* 无返回类型
* 可***重载***

```C++
// 一组重载的构造函数
class Triangular
{
    public:
        Triangular();  //默认构造函数
        Triangular(int len);
        Triangular(int len,int beginPos);
        ...
};
```

类的对象被定义后，***编译器会自动根据获得的参数挑选出应该被调用的构造函数***。



# 三、面向对象

> 关于基于对象和面向对象的编程范式区别，可以参考侯捷的C++视频教程。