类对象的拷贝初始化需要用到拷贝构造函数，与默认构造和传参构造不同，拷贝构造是用一个已有的相同类对象来初始化新建类对象。

可以看出，拷贝构造函数接受的参数应该是一个对象，具体点应该是对象引用，且需要加上 const 限定符来防止更改原对象。



> **数据成员有指针的类**



如果带指针的类，使用编译器自带的拷贝构造或拷贝赋值，只是拷贝了指针，只是指向同一个地方，并不是拷贝了原对象指向的数据。所以需要自定义拷贝构造和拷贝赋值函数。



【测试程序】

```c++
int main()
{
    String s1();           //默认构造
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
inline String::String(const char* cstr)
{
    if(cstr){   //指针非空
        m_string = new char[strlen(cstr) + 1];  //为数据成员分配空间
        strcpy(m_string,cstr);      //将参数字符串复制到数据成员字符串
    }
    else{       //指针为空，未指定初值
        m_string = new char[1];
        *m_string = `\0`
    }
}
```

