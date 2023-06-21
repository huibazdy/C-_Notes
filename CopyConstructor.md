类对象的拷贝初始化需要用到拷贝构造函数，与默认构造和传参构造不同，拷贝构造是用一个已有的相同类对象来初始化新建类对象。

可以看出，拷贝构造函数接受的参数应该是一个对象，具体点应该是对象引用，且需要加上 const 限定符来防止更改原对象。



> **数据成员有指针的类**



如果**带指针**的类，使用编译器自带的拷贝构造或拷贝赋值，只是拷贝了指针，只是指向同一个地方。所以**必须**要自定义**拷贝构造函数**和**拷贝赋值运算符**。否则一方面会造成内存泄漏，另一方面，两个指针同时指向一个数据，修改会互相影响，数据不安全。这种拷贝叫做**浅拷贝**。编译器默认执行浅拷贝。

**深拷贝**是先根据被拷贝的对象数据成员大小分配空间，再进行数据复制。



【测试程序】

```c++
int main()
{
    String s1();           //普通构造函数
    String s2("hello");    //普通构造
    String s3(s1);         //拷贝构造
    cout<<s3<<endl;        //重载 <<
    s3 = s2;               //拷贝赋值（也是一种拷贝构造），实质上是重载了 =
    cout<<s3<<endl;
}
```



【类头文件】

* 数据成员怎么设计？

    需要具有动态性，而不是一个定长的字符数组，因此考虑用指针来实现数据的绑定。

* 构造函数怎么设计？



```c++
#ifndef _MYSTRING_
#define _MYSTRING_

class String
{
public:
    String(const char* cstr = 0);          //普通构造
    String(const String& str);             //拷贝构造
    String& operator=(const String& str);  //拷贝赋值
    ~String();  //析构函数
    char* get_str() const {return m_string;} //获取数据成员
private:
    char* m_string;   //数据成员是指向字符串的指针
};

#endif
```



【类实现】

```c++
inline String::String(const char* cstr)   //普通构造函数的实现
{
    if(cstr){   //传进来的指针非空
        m_string = new char[strlen(cstr.m_string) + 1];  //为数据成员分配空间
        strcpy(m_string,cstr);      //将参数字符串复制到数据成员字符串
    }
    else{       //传进来的指针为空，即未指定初值
        m_string = new char[1];     //分配一个字节空间
        *m_string = `\0`;           //存储一个字符串结束符
    }
}

//拷贝赋值运算符

inline String::~String()
{
    delete[] m_string;
}


```



类中有指针数据成员，需要在构造函数中使用 **new** 动态分配，同时在析构函数中使用 **delete** 释放动态分配的内存。



> new 创建类对象

```c++
{
    ...
    String s1();
    String s2("hello");
    String* p = new String("hello");
    delete p;
    ...
}
```

在离开作用域时，s1、s2 都会调用析构函数释放内存空间。





> 拷贝赋值

拷贝赋值运算符`=`两边是两个对象，实际赋值的步骤应该如下：

1. 将左边对象清空
2. 创建出于右边对象相等大小的空间
3. 赋值

使用：

```c++
s2 = s1;
```

定义：

```c++
inline String& String::operator=(const String& str)
{
    if(this == &str)    //检查是否是自我赋值，自己赋值给自己，两个指针是否指的是同一个东西
        return *this;
    
    delete[] m_string;
    m_string = new char[strlen(str.m_string)+1];  //加 1 的原因是 strlen() 结果不包括结束符
    strcpy(m_string,str.m_string);
    
    return *this;
}
```

进行自我赋值检查的原因有两个：

1. 提高效率，，减少不必要操作；
2. 防止出错，如果是自我赋值，下面三个步骤是走不通的