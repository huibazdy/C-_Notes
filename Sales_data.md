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





## Sales_data类

Sale_data.h

> Sales_data类第一版



```C++
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



> Sales_data类四个不同的构造函数



```C++
struct Sales_data
{
    //新增的构造函数
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
