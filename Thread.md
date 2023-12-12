# Thread

简单多线程例子：

```c++
#include <iostream>
#include <thread>
using namespace std;

void foo(int n) {
    cout<<n<<endl;
}

int main()
{
    for(int n = 0; n < 5; n++){
        thread t(foo,n);    // 创建线程
        t.detach();         // 该线程在后台允许，无需等待该线程完成，继续执行后续语句
    }
    getchar();
    return 0;
}
```

运行结果：

![image-20231205160034097](https://raw.githubusercontent.com/huibazdy/TyporaPicture/main/image-20231205160034097.png)



结果和顺序执行区别很大，为什么会显示这个结果？如果我们创建的五个线程分别命名：

* t0：打印 0 和换行符；
* t1：打印 1 和换行符；
* t2：打印 2 和换行符；
* t3：打印 3 和换行符；
* t4：打印 4 和换行符；

实际结果是 t0 还没来得及打印换行符就切换到 t3 ，t3 运行结束后切换到 t1，然后执行 t2，最后执行 t4 。



根因：控制台是由 OS 管理的系统资源，只有一个，被这五个线程所共享。



> 应用程序、进程、线程的关系

* 每个应用程序至少有一个进程
* 每个进程至少有一个主线程，除主线程外可以创建多个线程
* 每个线程都有一个入口函数，入口函数返回退出，该线程也会退出
* 主线程的入口函数是 `main()` 函数



> C++ 线程管理类：`std::thread`

通过 thread 类，可以实现线程的：创建、启动、挂起、结束等操作。



* 启动线程

    只要创建一个`std::thread`对象，就会启动一个线程，并通过该对象来管理该线程

    ```c++



## 线程上下文

每个线程都自己的线程上下文，包括：

1. 唯一线程 ID：Thread ID，tid
2. 栈
3. 栈指针
4. 程序计数器
5. 通用目的寄存器
6. 条件码

所有运行在一个进程中的线程共享该进程的虚拟地址空间（代码、数据、堆、共享库、打开的文件）。



一个线程的上下文比一个进程的上下文小得多。



和一个进程相关的线程组成一个对等（线程）池，独立于其他线程创建的线程。



主线程和其他线程的区别是：它总是进程中第一个运行的线程。





```c++
#include <iostream>
#include <thread>

int hello() {
    std::cout<<"Hello Concurrence World"<<std::endl;
}

int main()
{
    std::thread t1(hello);
    t1.join();              // 该调用会令主线程等待子线程 t1
    return 0;
}
```

注意第 11 行，如果不加这一句，起始线程可能不等待新线程结束机会终止程序，更极端的情况是新线程还没启动就终止了程序。



每个 C++ 程序至少会包含一个线程（起始线程），就是 main 函数（main函数返回时，程序退出）。之后可以创建其他线程，即以其它函数为入口（入口函数返回时，现成结束）。这些新线程与起始线程并发运行。



## 启动线程

> 通过构建`std::thread`对象，管控线程的启动过程

最简单的任务就是运行一个一般的函数，复杂任务：例如函数对象，运行过程中经由某种系统消息协调，收到信号后才会停止。

只要通过 C++ 标准库来启动线程，都需要构造 `std::thread`对象。

```c++
#include <iostream>
#include <thread>

void do_work();
std::thread task1(do_work);
```



函数对象：

```c++
class CalcuSum {
public:
    double operator()(int a,int b,int c) const {
        return (double)(a + b + c) / 3;
    }
};

CalcuSum c;
std::thread task1(c);
```





> 启动线程后是等待它结束（汇合）还是任由它独自运行（分离）？

一个极端情况是：等到线程对象被销毁还没有指定，`std::thread`对象的析构函数会调用`std::terminate()`来终止整个程序。

我们只需要在线程对象被销毁前做决定即可避免这种情况发生。