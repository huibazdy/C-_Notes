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

> 在 C++ 中，当且仅当使用基类的***引用***或***指针***调用一个虚函数时将发生动态绑定。



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





## 抽象基类

如果存在多种折扣方案，例如：

1. 超过数量折扣
2. 超过数量 a 但是不能超过上限 b 否则按原价
3. 超过数量 c 后所有书籍都享受折扣

可以看出每一种策略都存在一个购买量和一个折扣值，可以定义一个折扣信息的类：

```c++
class DiscountInfo {
private:
    int sum;
    double disc;
};
```

在之前定义基类时，有一个 `totalCost()` 函数，但是对于折扣信息类来说，这个函数由于没有具体打折策略所以没有实际意义。所以可以使其继承基类中的该函数。

我们不希望用户创建一个 DiscountInfo 对象，因为单独存在的这个对象没有意义。因为它表示打折的通用概念（购买量以及折扣信息）。

解决方式：将 `totalCost()` 定义为纯虚函数（pure virtual），借此告诉用户该函数没有实际意义，且纯虚函数无须定义（如果要定义，在类外定义）。具体做法是：在函数体的位置写 `= 0` 即可。

至此，该类的 `totalCost()` 函数有两点需要注意：

1. 继承自基类；
2. 纯虚函数；

```c++
class DiscountInfo : public BookBase {
public:
    DiscountInfo() = default;
    DiscountInfo(const std::string& book, double price, std::size_t qty, double disc):
    			BookBase(book,price),  // 将前两个构造函数参数传递给 BookBase 基类的构造函数
    			quantity(qty),discount(disc) {}
    double totalCost(std::size_t) const = 0;
protected:
    std::size_t quantity;       // 折扣购买量
    double discount;            // 折扣
};
```



> **含有纯虚函数的类是抽象基类**

抽象基类负责定义接口，后续其他类可以覆盖该接口



为三种不同的折扣策略设计三种不同的类：

```c++
class Strategy1 {
    ...
};

class Strategy2 {
    ...
};

class Strategy3 {
    ...
};
```



先来实现第一种类，也就是***重写***最初定义的派生类：

```c++
class Strategy1 : public DiscountInfo {
public:
    Strategy1() = default;
    Strategy1(const std::string& book, double price, std::size_t qty, double disc):
    		Discount(book,price,qty,disc) {}
    double totalCost(std::size_t qty) const override;
};
```

直接基类是 `DiscountInfo` 这个抽象类，间接基类是 `BookBase` 。



> **关于构造函数的调用顺序**



`Strategy1` 类没有自己的数据成员，它的构造函数将实参传递给它的直接基类 DiscountInfo ，之后 DiscountInfo 的构造函数继续调用它的直接基类 BookBase 的构造函数来初始化 booNo 以及 price 成员，当这个构造函数结束后就开始运行 DiscountInfo 的构造函数并初始化 quantity 和 discount 成员，最后运行 Strategy1 的构造函数（还函数无须执行实际的初始化或其他工作）。



> **关于 `protected`**



* 类的用户不可访问
* 对于派生类的成员与友元可以访问
* 派生类的成员或友元只能通过派生类对象访问基类的 `protected` 成员

关于第三条规则，示例如下：

```c++
class Base {
	...
protected:
    int n;
	...
};

class Derived : public Base {
    friend void f1(Derived&);    // 通过派生类对象，可以访问基类成员 Derived::n
    friend void f2(Base&);       // 通过基类对象，不能访问基类成员 Base::n
    int m;
};

void f1(Derived &s) {s.m = s.n = 0;}  //正确，f1 可以访问 protected 成员
void f2(Base &r) {r.n = 0;}           //错误，f2 不能访问基类protected成员
```



> **为什么要将基类的析构函数声明为虚函数？**



设想这样一种情形：

```c++
class A {
public:
    ...
    ~A() = default;
};

class B : public A {
public:
    ...
    ~B() = default;
};
```

如果将一个基类指针绑定到派生类上，再用 delete 来通过该指针来删除对象，此时执行的是哪个析构函数？答案是：会产生未定义行为。然而实际上，我们意图删除的是派生类 B 的对象。

因为需要编译器清楚知道执行哪个析构函数，所以要将基类的析构函数声明为虚函数。

```c++
class A {
public:
    ...
    virtual ~A() = default;
};
```

这样就能保证在delete基类指针时运行正确的析构函数版本。



> **一个类如果需要析构函数，那么就需要拷贝和赋值操作，基类是例外**





# 拷贝、赋值、移动、销毁

构造函数用来控制在初始化对象时做哪些事情。

除此外，还有一些特殊的成员函数来控制对象的拷贝、赋值、移动或销毁。

## 拷贝构造函数

> 用同类型的对象***初始化***本对象时做什么？



```c++
class A {
public:
    A();         // 默认构造函数
    A(const A&); // 拷贝构造函数
    ...
};
```



* 第一个参数必须是引用
* 通常不应该是 explicit



拷贝初始化与直接初始化的区别：后者是要求编译器匹配参数相同的构造函数，前者要求编译器将右侧对象拷贝到正在创建的对象中（若需要还得进行类型转换）。

```c++
// 直接初始化
string s1(10,'.'); 
string s2(s1);

// 拷贝初始化
stirng s3 = s1;
string s4 = "Hi!";
string s5 = string(5,'9');
```



## 移动构造函数

> 用同类型的对象***初始化***本对象时做什么？



## 拷贝赋值运算符

> 将一个同类型对象***赋予***另一个对象时做什么？



## 移动赋值运算符

> 将一个同类型对象***赋予***另一个对象时做什么？



## 析构函数

> 此类型对象***销毁***时做什么？







【***难点***】

1. 什么时候需要定义这些拷贝控制操作？
