# exception

> 异常处理机制为检测异常和处理异常服务。

C++ 中的异常处理包括：

* **throw**

    用于异常检测，说明遇到了无法处理的问题，可以理解为引起异常

* **try & catch**

    用于异常处理，try 开始，并以一个或多个 catch 结束。try 语句块中是之前抛出的异常，catch 语句块处理异常

* 一套异常类

    在 throw 表达式和相关 catch 语句之间传递具体异常信息



例如我们自己定义一个除法运算：

```c++
double my_division(const T& a, const T& b) {
    double ans = a / b;
    if(b == 0)
        throw();
}
```



典型 try 语句块：

```c++
try {
    program-statements
} catch (exception-declaration) {
    handler-statements
} catch (exception-declaration) {
    handler-statements
} // ...
```

选中了一个 catch 语句后，执行相对应的块，catch 一旦完成，程序直接会跳过整个异常处理模块向后继续执行。



## 标准异常

C++ 标准库定义了一组类，用于报告标准库函数遇到的问题。分别定义在 4 个头文件中：

* *`<exception>`*

    定义了最通用的异常类 exception，但是只报告异常发生，不提供额外信息

* *`<stdexcept>`*

    包含几个常用异常类

* *`<new>`*

    定义了 bad_alloc 异常类型

* *`<type_info>`*

    定义了 bad_cast 异常类型