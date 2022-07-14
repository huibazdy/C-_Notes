# C++类

> 书店程序引入**自定义数据结构**来表示销售数据。C++通过定义类来定义自己的数据结构。类定义了一个**类型**，以及与之关联的**一组操作**。C++的目标是像

## Sals_item类

Sales_item有一个名为isbn的成员函数，且支持+、=、+=、<<、>>运算符。

* **isbn函数**：从一个Sales_item对象中提取ISBN号
* **输入**运算符（>>）：读Sales_item类型的对象
* **输出**运算符（<<）：写Sales_item类型的对象
* **加法**运算符（+）：将





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

