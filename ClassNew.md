## Sales_item

> 由书店程序引入自定义数据结构的需求，从而引入***class***概念。
>
> Sales_item需要具备以下功能：
>
> * 声明
> * 输入输出（`>>`、`<<`）
> * 提取这种类型变量的ISBN号
> * 赋值（`=`）
> * 加法（`+`）
> * 复合赋值（`+=`）



基本使用

```c++
#include<iostream>
#include"Sales_item.h"

int main()
{
    Sales_item book1;                  //声明一个Sales_item变量
    std::cin>>book1;                   //从标准输入读取某个交易数据，存入变量 
    std::cout<<book1<<std::endl;       //打印变量值
    
    Sales_item book2;
    Sales_item book3;
    std::cin>>book2>>book3;             //读取两本书的交易记录
    std::cout<<book2+book3<<std::endl;  //打印两本书交易记录的和
    
    return 0;
}
```



成员函数

> 两笔书籍交易记录，若不是对一个一个ISBN号是不能直接 相加的，这就需要我们实现一个提取Sales_item类型变量ISBN号的函数`isbn()`。这个函数是Sales_item类的一部分，称为成员函数（或方法）。

```C++
#include<iostream>
#include"Sales_item.h"

int main()
{
    Sales_item item1,item2;
    std::cin>>item1>>item2;
    if(item1.isbn() == item2.isbn())  //通常以类对象的名义调用成员函数
    {
        std::cout<<item1+item2<<std:endl;
        return 0;
    }
    else
    {
        std::cerr<<"Data must refer to same ISBN"<<std::endl;
        return -1;
    }
}
```



书店程序

```c++
#include<iostream>
#include"Sales_item.h"

int main()
{
    Sales_item nextItem;    //保存下一条交易记录的变量
    if(std::cin>>nextItem)  //读取第一条交易记录，保证有交易数据可以处理
    {
        Sales_item total;   //保存交易和的变量
        while(std::cin>>total)
        {
            if(nextItem.isbn() == total.isbn()) //如果我们仍在处理相同的书
            {
                nextItem += total;   //更新销售总额
            }
            else    
            {
                std::cout<<nextItem<<std::endl;  //打印前一本书的结果
                nextItem = total;                //nextItem现在表示下一本书的销售额
            }
        }
        std::cout<<nextItem<<std::endl;
    }
    else
    {
        std::cerr<<"No data?!"<<std::endl;  //没有输入，警告
        return -1；
    }
    return 0;
}
```





## Sales_data

> `Sales_item`类将书籍的ISBN号、销量、销售额等数据，并提供isbn函数、`>>`、`<<`、`+`、`=`、`+=`等操作。（实际上就是一个数据结构：包括一组组织起来的数据以及使用它们的方法）
>
> 到目前为止，我们掌握的知识还不足以写出完整的Sale_item类（还没有讲解运算符重载）。
>
> 可以尝试写一个简单的类，来组织数据（即数据成员），并实现基本操作，将其定义为`Sale_data`类。



```c++
struct Sales_data       //Sales_data类内部有三个数据成员
{
    std::strig bookNo;  //ISBN号用string类型存储
    unsigned units_sold;
    double revenue;
};
```

【**注意**】

* 类的每个对象都有一份数据成员拷贝，修改一个对象的数据成员，不会影响类的其他对象

* 创建对象时可以提供一个**类内初始值**来初始化数据成员，没有初始值的成员被**默认初始化**

    （上述定义中未提供类内初始值，故分别被默认初始化为空字符串和两个0）

* 在学习class关键字之前，使用struct来定义自己的数据类型



Sales_data类我们没有定义任何操作，所以需要我们自己编码实现输入`>>`、输出`<<`、相加`+`等功能。（假设已知Sales_data类定义于Sales_data.h头文件内）

```c++
#include<iostream>
#include<string>            //string支持的操作有：读入(>>)、写出(<<)、比较(==)
#include"Sales_data.h"

int main()
{
    Sales_data book1,book2;
    double price = 0;
    std::cin>>book1.bookNo>>book1.units_sold>>price;
    book1.revenue = book1.units_sold * price;
    std::cin>>book2.bookNo>>book2.units_sold>>price;
    book2.revenue = book2.units_sold * price;
    //检查两笔交易的ISBN号是否相同
    if(book1.bookNo == book.bookNo)
    {
        unsigned totalCnt = book1.units_solt + book.units_sold;
        double totalRevenue = book1.revenue + book2.revenue;
        //输出：ISBN、总销量、总销售额、平均价格
        std::cout<<book1.bookNo<<" "<<totalCt<<" "<<totalRevenue<<" ";
        if(totalCnt != 0)
            std::cout<<totalRevenue/totalCnt<<std::endl;
        else
            std::out<<"No sales!"<<std::endl;
        return 0;
    }
    else
    {
        std::cerr<<"Data must refer to the same ISBN"<<std::endl;
        return -1;
    }
}
```



> ***头文件***

若要在不同文件中使用同一个类，类的定义需要保持一致，故类一般定义在头文件中。

* 头文件中一般包含只能被定义一次的实体（类、const变量、constexpr变量）
* Sales_data类中包含string数据成员bookNo所以Sales_data头文件中需要包含<string>
* 在使用Sales_data类的程序中为了操作bookNo数据成员，需要再次包含<string>
* 头文件一旦修改，相关源文件需要重新编译



> ***预处理器***

预处理器是确保头文件被多次包含仍能安全工作的技术。C++中预处理器是集成C而来。



头文件保护符（header guard）

* 头文件保护符依赖于预处理变量
* 预处理变量有两种状态：已定义和未定义
* `#define：将一个名字设定为预处理变量
* `#ifdef`：当且仅当预处理变量已定义
* `#ifndef`：当且仅当预处理变量未定义
* 一系列检查结果为真，则执行后续操作直到遇到`#endif`指令为止



使用以上功能就能有效防止重复包含发生：

```c++
#ifndef SALES_DATA_H
#define SALES_DATA_H

#include<string>
struct Sales_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

#endif
```



> ***重新设计Sales_data类***

【**功能**】

我们最终的目的是令Sales_data类和Sales_item类一样，支持`>>`、`<<`、`+`、`=`、`+=`等运算符，且支持`isbn()`成员函数。



【**成员**】

* `+`：不作为成员，用`add()`函数实现两个对象的加法
* `>>`、`<<`：不作为成员，用`read()`、`print()`函数实现从输入输出流读写Sales_data对象
* `+=`：作为成员函数，用`combine()`成员函数来实现，将以一个Sales_data对象加到另一个对象上
* `isbn()`函数返回Sales_data对象的ISBN号



【**定义新类**】

```c++
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include<string>

struct Sales_data
{
    std:string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
    
    std::string isbn() const {return bookNo;}  //实现isbn()成员函数，返回string对象
    Sales_data& combine(const Sales_data&);    //实现复合赋值运算符：+=
    double avg_price() const;                  //额外添加返回平均售价的成员函数
};

//Sales_data的非成员接口函数
Sales_data add(const Salas_data&,const Salas_data&);    //实现加法运算符：+
std::istream& read(std::istream&,Sales_data&);          //实现输入运算符：>>
std::ostream& print(std::ostream&,const Slaes_data&);   //实现输出运算符：<<

#endif
```

注意：定义在类内的函数是***隐式inline***函数。



> ***this指针***

关注重写的`isbn()`函数使用：

```c++
Sales_data book1;
std::cout<<book1.isbn()<<std::endl;
```

* 调用成员函数时，实质上是类的某个对象在调用它。
* 在代码中，实际上`isbn()`隐式地指向调用该函数的对象（即：book1）的数据成员（book1.bookNo）
* `isbn()`返回bookNo时，实际上返回的是book1.bookNo
* 成员函数通过一个名为`this`的额外***隐式参数***来访问调用它的那个对象（编译器将book1***对象的地址***传递给isbn的隐式形参this，可以理解为编译器将该调用重写为了一下形式：`Sales_data::isbn(&book1)`）



定义一个返回this对象的函数：

```c++
Sales_data& Sales_data::combine(const Sales_data &rhs)
{
    units_solt += rhs.units.sold;
    revenue += rhs.revenue;
    return *this;
}
```



>  ***const成员函数***

* const限定符在成员函数参数列表与函数体之间
* const在这里表示this是一个指向常量的指针
* 把this设置为指向常量的指针有助于提高函数的灵活性
* 像这样使用const的成员函数称作***常量成员函数***



> ***类相关非成员函数***

类作者常常需要定义一些辅助函数，如：`read`、`print`、`add`等。尽管这些函数定义的操作属于类接口的组成部分，但不属于类本身。

此类函数在概念上属于类，但不定义在类中，一般与类声明放在同一个头文件中。

```c++
//read函数：从给定流中将数据读到给定对象里去
istream &read(istream &is,Sales_data &item)
{
    double price = 0;
    is>>item.bookNo>>item.units_solf>>price;
    item.revenue = price * item.units_sold;
    return is;
}
//print函数：将给定对象的内容打印到给定流中
ostream &print(ostream &os,const Sales_data &item)
{
    os<<item.isbn()<<" "<<item.units_sold<<" "
      <<item.revenue<<" "<<item.avg_price();
    return os;
}
```

* `read`和`print`都各接受一个***IO类型的引用***作为参数，这是因为***IO类型不能被拷贝***，只能通过***引用传递***
* 因为读写会改变流的内容，故两个函数接受的都是普通引用，而非对常量的引用。



定义`add`函数：

```c++
Sales_data add(const Sales_data &lhs,const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
```



> ***构造函数***

* 构造函数是一个或几个特殊的成员函数
* 构造函数的任务是初始化类对象的数据成员
* 若类未显示定义任何构造函数，<u>*编译器*</u>会隐式创建一个默认构造函数，称为***合成的默认构造函数***（工作流程：若类内有初始值则用这些值初始化，否则执行默认初始化）

```c++
struct Sales_data
{
    //新增构造函数
    Sales_data() = default;  
    Sales_data(const std::string &s):bookNo(s){}
    Sales_data(const std::string &s,unsigned n,double p):
              bookNo(s),units_sold(n),revenue(p*n){}
    Sales_data(std::istream &);
    //之前已有成员
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    double avg_price()const;
    std::string units_sold;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```



> ***访问控制***

* `public`：该说明符后的成员可以在整个程序内被访问，public成员定义类的接口
* `private`：该说明符后的成员可以被类的成员函数访问，但不能被使用该类的代码访问

```c++
class Sales_data
{
 public:
    Sales_data() = default;  
    Sales_data(const std::string &s):bookNo(s){}
    Sales_data(const std::string &s,unsigned n,double p):
              bookNo(s),units_sold(n),revenue(p*n){}
    Sales_data(std::istream &);
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
 private:
    double avg_price()const{return units_sold ? revenue/units_sold : 0};
    std::string units_sold;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```



> ***友元***

由于Sales_data类的数据成员是private的，而属于类的接口但不是成员函数的`read`、`print`、`add`三个函数就无法正常编译了（因为它们的实现要访问类的数据成员）。

类可以允许其他类或函数访问非公有成员，方法是令其他类或函数称为它的***友元（friend）***。

如果类想让一个函数成为其友元，做法是在类定义中加上以`friend`关键字开始的函数声明语句：

```C++
class Sales_data
{
    friend Sales_data add(const Sales_data&,const Sales_data&);
    friend std::istrem &read(std::istream&,Sales_data&);
    friend std::ostream &print(std::ostream&,const Sals_data&);
    
    private:
        std::string bookNo;
        unsigned units_sold = 0;
        double revenue = 0.0;
    public:
        Sales_data() = default;  
        Sales_data(const std::string &s):bookNo(s){}
        Sales_data(const std::string &s,unsigned n,double p):
                  bookNo(s),units_sold(n),revenue(p*n){}
        Sales_data(std::istream &);
        std::string isbn() const {return bookNo;}
        Sales_data& combine(const Sales_data&);
};
```

【注意】友元声明只能出现在类定义的内部，但在类内出现的具体位置不限（一般在开始或结束位置集中声明）。



> ***相互关联的类***

* `Screen类`

    表示显示器中一个窗口。每个Screen对象有一个用于保存Screen内容的string类型成员和三个string::size_type类型的成员（分别表示光标位置以及屏幕高、宽）。

    除了定义数据成员外，类还可以定义某种类型在类中的别名，且别名存在访问限制：

    ```c++
    class Screen
    {
        public:
            typedef std::string::size_type pos;
        private:
            pos cursor;
            pos hight = 0,width = 0;
            std::string contents;
    };
    ```

* `Screen类的成员函数`

    为了使类更实用，我们添加一个构造函数使用户能定义屏幕尺寸和内容；添加两个成员负责光标移动和给定位置：

    ```c++
    class Screen
    {
        public:
            typedef std::string::type_size pos;
            Screen() = default;    //因为类有另一个构造函数，所以此默认构造函数是必须的
            Screen(pos ht,pos wd,char c):height(ht),width(wd),contents(ht*wd,c){}
            char get()const                        //读取光标处字符
                 {return contents[cursor];}        //隐式内联
            inline char get(pos ht,pos wd)const;   //显示内联
            Screen &move(pos r,pos c);             //能在之后被设为内联
        private:
            pos cursor = 0;
            pos height = 0,width = 0;
            std::string contents;
    };
    ```

    在类中，有些***规模较小***的函数适合被声明为***内联函数***。最好只在类外部显式说明inline，这样类更易理解。
    
    ```c++
    inline Screen &Screen::move(pos r,pos c)   //类外定义inline函数
    {
        pos row = r * width;                   //计算行的位置
        cursor = row + c;                      //在行内将光标移动
        return *this;                          //以左值形式返回对象
    }
    char Screen::get(pos r,pos c)const
    {
        pos row = r * width;
        return contents[row + c];
    }
    ```



> ***重载成员函数***

和非成员函数一样，成员函数也可以重载，只要参数数量或类型不同即可。



> ***可变数据成员***

有时希望修改类的某个数据成员，即使在一个const成员函数内。可以变量声明是加上`mutable`关键字来实现。

可变数据成员（mutable data member）永远不会是const，即使它是const对象的成员。



> ***友元再探***





> ***委托构造函数***

C++11新增了委托构造函数（delegating constructor）。委托构造函数**使用它所属类的其他构造函数**执行它自己的初始化过程。

在委托构造函数内，成员初始值只有唯一入口，就是类名本身，类名后紧跟圆括号括起来的参数列表，参数列表必须与类中的另一个构造函数匹配。

```C++
class Sales_data
{
    //非委托过早函数使用对应的实参初始化成员
    Sales_data(std::string s,unsigned cnt,double price):
        bookNo(s),units_sold(cnt),revenue(price*cnt){}
    //其余的构造函数全部委托给另一个构造函数
    Sales_data():Sales_data("",0,0){}
    Sales_data(std:string s):Sales_data(s,0,0){}
    Sales_data(std::istream &is):Sales_data(){read(is.*this);}
    //其他成员与之前版本一致
    ......
};
```



> ***静态成员***

* 静态数据成员为所有对象共享，存在于任何对象之外
* 静态成员函数不能声明为const，函数体也不能使用this指针
* 类的静态数据成员不应该在类内初始化
* 静态数据成员类似于全局变量，定义域任何函数之外，一旦被定义，存在于程序整个生命周期。
