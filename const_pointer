# const

## const 与指针

### 指向常量的指针

* 要想存储常量对象的地址，***必须使用***指向常量的指针。

    ```C++
    const double pi = 3.14;
    double *ptr = &pi;           //错误：ptr只是一个普通指针，不能指向常量对象
    const double *cptr = &pi;    //正确：cptr是一个常量指针，而且指向的类型是双精度常量
    ```

* 指向常量的指针***不能改变***其所指对象的值。

    ```c++
    const double pi = 3.14;
    const double *ptr = &pi;
    *ptr = 314;                  //错误：不能利用指向常量的指针来改变所指常量的值
    ```

* 指向常量的指针可以指向一个非常量对象，但***依然不能改变***所指非常量对象的值

    ```c++
    const double pi = 3.14;
    const double *ptr = &pi;
    double val = 100;
    ptr = &val;                  //正确：原本指向常量对象的指针可以改而指向非常量对象
    *ptr = 99;                   //错误：指向常量的指针，不能改变其指向的对象，无论该对象是否为常量
    ```



### const指针

指针是对象而引用不是，允许把指针像其他类型的对象一样定义为常量（把`*`放在`const `之前）。

* 常量指针（const pointer）必须初始化，且初始化完成后，它的值就不能改变

    ```c++
    int val = 1;
    int *const ptr = &val;            //将ptr一直指向val
    const double pi = 3.14;
    const double *const cptr = &pi;   //cptr是一个指向常量对象的常量指针   
    ```

* 常量指针本身是常量，并不意味不能通过它修改所指对象的值

    ```C++
    *ptr = 0;      //正确：ptr为指向普通对象的常量指针，可以修改所指对象的值
    *cptr = 314;   //错误：cptr为指向常量的常量指针，不能改变所指对象的值
    ```

    