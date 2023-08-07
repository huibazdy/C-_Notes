# vptr & vtbl

首先引入三个类之间的继承关系：

* **基类 A**

    ```c++
    class A
    {
    public:
        virtual void vfunc1();
        virtual void vfunc2();
        void func1();
        void func2();
    private:
        int m_data1;
        int m_data2;
    };

* **A 的派生类 B**

    ```c++
    class B:public A
    {
    public:
        virtual void vfunc1();  //默认继承 A::vfunc2()，重写 B::vfunc1()
        void func2();           //默认继承 A::func1()，重写 B::func2()
    private:
        int m_data3;            //默认继承 m_data1 与 m_data2
    };
    ```

* **B 的派生类 C**

    ```c++
    class C:public B
    {
    public:
        virtual void vfunc1(); //默认继承 B::vfunc2()，重写 C::vfunc1()
        void func2();          //默认继承 B::func1()，重写 C::func2()
    private:
        int m_data1;           //默认继承 B 中 m_data2、m_data2
        int m_data4;
    };
    ```

    

只要类带有虚函数，那么类对象的存储空间中就会带有一个虚指针（指向虚表），大小为 4 字节（32位机器）。也就是说，类对象大小就是数据成员所占空间加上 4 个字节。

vptr 指向 vtbl。

vtbl 中存放的都是函数指针，指向的是类涉及到的虚函数。



【例】指向对象的指针调用虚函数

```c++
C* p = new C;  //实例化一个 C 对象，让指针 p 指向它

```





> 关于继承与派生需要知道的基础知识

1. 派生类必须在其内部对所有需要重新定义的虚函数进行声明，需要在这样的函数之前加上**`virtual`**关键字或在形参列表后加上**`override`**关键字。
2. 基类将成员函数分为两类，一类是希望派生类进行重写（或覆盖）的函数，称为***虚函数***；另一类是希望派生类直接继承而不要进行改变的函数。
3. 基类的析构函数一般都声明为虚函数。
4. 根据引用或指针所绑定对象类型的不同，决定执行基类的函数版本还是执行某个派生类函数版本的机制称为***动态绑定***。可以理解为声明为 virtual 的函数，才有可能发生动态绑定（因为同名、重写）。
5. 

