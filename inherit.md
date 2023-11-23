基类：原价图书

```c++
class OriginalPriceBook
{
pubilic:
    std::string isbn() const;   // 返回书籍 ISBN 编号，因为不涉及派生类的特殊性，只需要定义一次
    virtual double sales_price(size_t n) const;  // 返回书籍的实际销售价格，与派生类相关，两个类都包含该函数
private：
    string m_isbn;
    double m_salesPrice;
};
```

基类希望派生类定义适合自己的版本的成员函数，就会将其声明为虚函数。



派生类：打折图书

```c++
class DiscountedBook : public OriginalPriceBook  // 继承于 OriginalPriceBook 类
{
public:
    double sales_price(size_t n) const override;
private:
    ...
};
```

关键字 public 使得完全可以把派生类的对象当成基类对象来使用。



## 动态绑定

通过动态绑定机制，可以用同一段代码分别处理基类对象和派生类对象。

例如，编写一个函数，当知道书籍信息和购买数量时，打印总费用

```c++
double printTotalCost(ostream &os, const OriginalPriceBook &item, size_t n)
{
    //根据实参类型决定调用 OriginalPriceBook::sales_price() 或 DiscountedBook::sales_price()
    double ret = item.sales_price(n);
    os << "ISBN: "<<item.isbn()<<" "
       << "Sold: "<<n<<" "
       << "Total Cost: "<<ret<<endl;
    return ret;
}
```

> 根据实际传入对象的类型决定执行哪个函数版本（基类或派生类），这就是**动态绑定（Dynamic Binding）**

例如：

```c++
OriginalPriceBook basic;
DiscountedBook bulk;

printTotalCost(cout,basic,20);  // 调用 OriginalPriceBook::sales_price()
printTotalCost(cout,bulk,20);   // 调用 DiscountedBook::sales_price()
```

在程序运行时，根据函数实参类型选择函数版本，因此动态绑定又称**运行时绑定（Run-time Binding）**。

> 在 C++ 中，当且仅当使用***引用***或***指针***调用一个虚函数时将发生动态绑定。



## 定义基类



```c++
class OriginalPriceBook
{
public:
    OriginalPriceBook() = default;
    OriginalPriceBook(const string &isbn, double price):
    				bookNo(isbn),salesPrice(price) {}
    virtual double totalCost(size_t n) const {return n * salesPrice;}
    virtual ~OriginalPriceBook() = default;
private:
    std::string bookNo;        // ISBN 号
protected:
    double salesPrice = 0.0;   // 不打折的价格
};
```



基类需要区分两种成员函数：

1. 希望派生类进行覆盖的函数；
2. 希望派生类直接继承不要改变的函数

前者定义为虚函数，当用引用或指针调用虚函数时，会执行动态绑定。



> **任何构造函数之外的非静态函数都可以是虚函数。**



基类区分两种数据成员：

1. 不希望被派生类访问的，即私有成员
2. 希望能被派生类访问的，即受保护成员，用 **`protected`** 修饰



## 派生类



> **派生类能访问共有成员，不能访问私有成员。**



```C++
class DiscountedBook : public OriginalPriceBook
{
public:
    DiscountBook() = default;
    DiscountBook(const std::string &isbn, double price, std::size_t n, double discount);
private:
    std::size_t minQuantity;    // 能打折的最低购买数量
    double discount;            // 达到打折数量后的折扣信息
};
```



> **派生列表中的说明符的作用是控制派生类从基类继承的成员对派生类用户是否可见**





## 虚函数



1. 派生类中函数***覆盖***某个基类中的虚函数时，virtual 关键字可省略；
2. 派生类中函数若覆盖某个集成来的虚函数，它的形参类型必须与被它覆盖的基类函数完全一致；
    1. 派生类中若定义了一个与基类虚函数同名但形参列表不同，也是合法的，编译器认定这是一个与基类函数完全独立的函数。这时，派生类函数***没有覆盖***掉基类中的版本。但是实际情况是，如果意图覆盖，却因为写错，写成了没有覆盖的情况，此时编译器是检查不出来的（所以调试这样的问题非常困难）。所以 C++11 标准中，意图覆盖虚函数的写法后面需要加上 `override` 关键来帮助编译器检查上述错误（此时编译器将报错）。



```c++
class A {
public:
    virtual void func1(int) const;
    virtual void func2();
};

class B : public A {
public: 
    void func1(int) const override;   // 正确，与基类形参列表匹配
    void func2(int) override;         // 错误，A 中没有形如 func2(int) 的函数，无法覆盖
};
```



关键字 `final` 可以避免某个函数之后被覆盖，否则会引发错误，例如：

```c++
class B : public A {
public:
    void func1(int) const final;     // 不允许后续其他类进行覆盖 func1(int)
    virtual void func2() override;
};

class C : public B {
public:
    void func1(int) const;          // 错误， B 已经将 func1 声明为 final 无法覆盖
    void func2();                   // 正确，覆盖从间接基类 B 继承来的 func2   
};
```

第 9 行中因为形参列表一致，所以解析为意图覆盖，所以会报错。