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