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
        virtual void vfunc1();
        void func2();
    private:
        int m_data3;
    };
    ```

* **B 的派生类 C**

    ```c++
    ```

    