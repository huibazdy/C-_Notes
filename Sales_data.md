# C++类

> 书店程序引入**自定义数据结构**来表示销售数据。C++通过定义类来定义自己的数据结构。类定义了一个**类型**，以及与之关联的**一组操作**。C++的目标是像使用内置类型一样去使用类。



## Sals_item类

Sales_item有一个名为isbn的成员函数，且支持+、=、+=、<<、>>运算符。

* **isbn函数**：从一个Sales_item对象中提取ISBN号
* **输入**运算符（>>）：读Sales_item类型的对象
* **输出**运算符（<<）：写Sales_item类型的对象
* **加法**运算符（+）：将两个Sales_item对象相加，但两个对象必须有相同的ISBN号。加法结果是一个新的Sales_item对象
* **复合赋值**运算符（+=）：将一个Sales_item对象加到另一个对象上

从标准输入读入一个Sales_item对象：

```C++
#include<iostream>
#include"Sales_item.h"
int main()
{
    Sales_item book1;   //定义一个名为book1的Sales_item对象
    std::cin>>book1;
    std::cout<<book1<<std::endl;
    return 0;
}
```

Sales_item对象加法：

```C++
#include<iostream>
#include"Sales_item.h"
int main()
{
    Sales_item book2,book3;
    std::cin>>book1>>book2;  //读取一对交易记录
    if(book1.isbn() == book2.isbn())
    {
        std::cout<<book1+book2<<std::endl; //打印它们的和
        return 0;
    }
    else
    {
        std::cerr<<"Data must refer to same ISBN"<<std::endl;
        return -1;
    }
}
```



**书店程序**，我们需要从一个文件中读取销售记录，生成每本书的销售报告，显示出售册数、总销售额、平均售价。假定每个ISBN书号的销售记录在文件中是聚在一起保存的。书店程序会将每个ISBN的数据合并起来，存入名为total的变量中。名为trans的变量保存读取每条销售记录。如果trans和total指向相同的ISBN，则更新total的值。否则也打印total的值，并将其重置为刚刚读取的数据。

```C++
#include<iostream>
#include"Sales_item.h"
int main()
{
    Sales_item total;  //保存某个ISBN的销售数据之和，从第一条销售数据开始
    if(std::cin>>total)
    {
        Sales_item trans; //临时保存读入的销售记录，读取剩下的所有销售记录
        while(std::cin>>trans)
        {
            if(trans.isbn() == total.isbn()) //仍在处理同一本书
                total += trans;
            else
            {
                std::cout<<total<<std::endl;
                total = trans;  //total现在表示下一本书（下一个ISBN号）的销售总数据
            }
        }
        std::cout<<total<<std::endl; //打印最后一个ISBN号（最后一本书）的销售数据总和
    }
    else
    {
        std::cerr<<"No data?!"<<std::endl;  //警告用户没有输入
        return -1;
    }
    re	
}
```

**Sales_item.h头文件**

```C++
#ifndef SALESITEM_H
#define SALESITEM_H

#include"version_test.h"

#include<iostream>
#include<string>

class Sales_item
{
    friend std::istream& operator>>(std::istream&, Sales_item&);
    friend std::ostream& operator<<(std::ostream&, Slaes_item&);
    friend bool operator<(const Sales_item&, const Sales_item&);
    friend bool operator==(const Slaes_item&, const Slaes_item&);
    
public:
#if defined(IN_CLASS_INITS) && defined(DEFAULT_FCNS)
    Sales_item() = default;
#else
    Sales_item():units_sold(0),revenue(0.0){}
#endif
    Sales_item(const std::string &book):
              bookNo(book),units_sold(0),revenue(0.0){}
    Sales_item(std::istream &is){is >> *this;}
};
```



---



















## Sales_data类

> **最初的Sales_data类**



```C++
//此时只有数据成员，类定义存在于头文件"Sales_data.h"
struct Sales_data{
    std::string bookNo;       //某本书ISBN编号
    unsigned units_sold = 0;  //某本书销量
    double revenue = 0.0;     //某本书的总销售额
};
```

【==**头文件保护符**==】

C++程序会用到一种预处理功能是头文件保护符（header guard），它是一种**特殊的预处理变量**，用来防止头文件被重复包含。

【==**预处理变量**==】

`#define`指令把一个名字设定为**预处理变量**。预处理变量有两种状态：**已知**和**未定义**。`#ifdef`指令判断预处理变量是否已经定义，当且仅当预处理变量已定义时为真；`#ifndef`指令判断预处理变量是否未定义，当且仅当预处理变量未定义时为真。一旦检查结果为真，则执行后续操作直至遇到`#endif`指令为止。

使用这些功能就能有效**防止头文件重复包含**的发生。

```C++
/***头文件："Sales_data.h"***/
#ifndef SALES_DATA_H    //检查预处理变量SALES_DATA_H是否未被定义，当且仅当未被定义才执行后续指令
#define SALES_DATA_H    //指定SALES_DATA_H为头文件保护符
#include<string>
struct Saless_data{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif //执行到此处，预处理变量SALES_DATA_H将变为已定义，该头文件才会被拷贝到程序中去
       //后续若再次包含头文件Sales_data.h，则#ifndef检查结果为假，编译器将忽略#ifndef到#endif之间内容
```

【==**注意事项**==】

整个程序中的预处理变量（包括头文件保护符）必须唯一，通常做法是基于头文件中**类的名字来构建保护符**的名字。同时，为避免与程序中其他实体重名，一般将**预处理变量全部大写**。

```C++
/***官方头文件***/
#ifndef SALES_DATA_H
#define SALES_DATA_H

#include"Version_test.h"

#include<string>

struct Sales_data
{
    std::string bookNo;
#ifdef IN_CLASS_INITS
    unsigned units_sold = 0;
    double revenue = 0.0;
#else
    unsigned units_sold;
    double revenue;
#endif
};
#endif
```







> **改进的Sales_data类**

不同于Sales_item类（可以通过其接口来使用其对象，且不能访问其数据成员），Sales_data不是抽象数据类型（它允许用户直接访问其数据成员，并且要求用户来编写操作）。

要想把Sales_data变成抽象数据类型，需要为其定义一些操作供类的用户使用。一旦定义了它自己的操作，就可以实现对数据成员的封装（隐藏）。

我们的目的是使Sales_data支持与Sales_item类相同的操作集合，包括：

* 从对象中获取ISBN号：`isbn()`
* 输入输出运算符读写对象：`>>`、`<<`
* 用一个对象给另一个对象赋值：`=`
* 将两个对象相加生成一个新的对象：`+`
* 将一个对象加到另一个对象上：`+=`

暂时不考虑自定义运算符（即：运算符重载），以函数形式来实现这些操作：

* 获取ISBN号：`isbn()`
* 从istream读入数据到Sales_data对象中：`read()`
* 将Sales_data对象中的值输出到ostream：`print()`
* 将两个对象相加生成一个新的对象：`add()`
* 将一个对象加到另一个对象上：`combine()`



如何使用这些即将被实现的函数？参考书店程序的实现：

```C++
/*注意：默认该销售信息表格ISBN相同书本销售条目的已经聚合在一起*/
Sales_data total;              //用来保存求和结果的变量
if(read(cin,total)){           //读入第一本书
    Sales_data temp;           //temp变量保存下一条交易数据，用来遍历交易表格
    while(read(cin,temp)){
        if(total.isbn() == temp.isbn()){
            total.combine(temp);
        } 
        else{
            print(cout,total)<<endl;
            total = temp;      //处理下一本书
        }    
    }
    print(cout,total)<<endl;
}
else{
    cerr<<"No data?!"<<endl;   //没有任何输入信息的时候，通知用户
}
```



开始改造Sales_data类：

```C++
struct Sales_data{
    //数据成员没变
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
    //接口添加
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
};
//非成员接口函数
Sales_data add(const Sales_data&,const Sales_data&);
std::istream &read(std::istream&,Sales_data&);
std::ostream &print(std::ostream&,const Sales_data&);
```



定义成员函数：（`isbn()`定义在类内，`combine()`以及`avg_price()`定义在类外）



【==**引入this**==】

观察isbn函数的实现发现函数体内只有一条return语句，用来返回Sales_data对象的bookNo数据成员。实质上是`total.isbn()`语句隐式地返回**total.bookNo**。

成员函数通过一个名为this的额外隐式参数来访问调用它的那个对象。

当我们调用一个成员函数时，用请求该函数的对象地址初始化this。

在这个例子中，编译器把total的地址传递给了**isbn函数的隐式形参this**，可以等价为编译器将该函数调用重写为：

`Sales_data::isbn(&total)`

调用isbn成员函数时，传入了调用对象total的地址。

对任何类成员的直接访问都被看作this的隐式引用，当isbn使用bookNo时，它隐式地使用this指向的对象，于是成员函数的定义可以写成：

`std::string isbn() const {return this->bookNo;}`

因为this的目的总是指向调用成员函数的“这个”对象，所以它是一个常量指针，不允许改变this中保存的地址。



【==**引入const成员函数**==】

isbn函数后的const作用是修饰this指针的类型。

默认情况下，this的类型是指向类类型非常量版本的常量指针。





```C++
struct Sales_data
{
    //新增四个构造函数
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s){}
    Sales_data(const std::string &s,unsigned n,double p)
              :bookNo(s),units_sold(n),revenue(p*n){}
    Sales_data(std::istream &);
    //之前已有的其他成员
    std::string isbn() const {return bookNo;}
    Sales_data & combine(const Sales_data);
    double average_price() const;
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```

​	
