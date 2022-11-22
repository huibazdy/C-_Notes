## OOP概述

面向对象程序设计的核心思想是：数据抽象、继承、动态绑定。

* ***数据抽象***：
* ***继承（inheritance）***
    * 可以定义相似关系，并对其相似关系建模
    * 联系在一起的类构成一种层次关系，通常在层次关系根部有一个基类（base class），继承得到的类称为派生类（derived class）
    * 基类负责定义层次关系中所有类的共有成员
    * 派生类定义各自特有成员
* ***动态绑定（dynamic binding）***
    * 可以在一定程度上忽略相似类型的区别，而以更统一的方式使用他们的对象



对不同种类的书籍进行不同的销售策略，首先定义基类`Quote`来表示按原价销售的书籍，由其派生出`Bulk_quote`的子类表示打折销售的书籍。

考虑下列两个成员函数：

* `isbn()`：返回书籍的ISBN号，不是派生类特有的（不会因为派生类或基类的区别而表现区别行为），只需包含在基类中
* `net_price(size_t)`：返回书籍的实际销售价格，会根据派生类和基类而表现出明显不同，因此需要同时包含在基类和派生类中。



基类希望某些函数派生类自定义实现各自的版本，会将其声明为***虚函数***（`virtual`）：

```c++
class Quote
{
public:
    std::string isbn() const;
    virtual double net_price(std::size_t) const;
};
```



派生类需要通过使用***类派生列表***指明它是从哪个（哪些）基类继承而来：

```c++
class Bulk_quote:public Quote  //类派生列表
{
public:
    double net_price(std::size_t) const override;  
    //C++11允许通过override关键字显式注明改写基类虚函数
};
```



通过***使用动态绑定***，我们能用同一段代码处理Quote和Bulk_quote对象。例如，已购书籍和购买数量都已知情况下，下面函数打印总费用：

```C++
double print_total(ostream &os,const Quote &item,size_t n)
{
    //根据传入item形参的类型调用Quote::net_price或Bulk_quote::net_price
    double ret = item.net_price(n);
    os<<"ISBN: "<<item.isbn()  //调用Quote::isbn()
      <<"#sold: "<<n<<" total due: "<<ret<<endl;
    return ret;
}
```



