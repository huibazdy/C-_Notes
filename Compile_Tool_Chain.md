## 一、GCC编译过程

编译过程如下图（[图片来源](https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html)）：

<img src="https://raw.githubusercontent.com/huibazdy/TyporaPicture/main/202211121120562.png" alt="image-20221112112054425" style="zoom:67%;" />

### 1. 预编译（Preprocessing）

* ***输入***：源代码，例如`.c`/`.cpp`/`.h`等源文件
* ***输出***：中间文件，`.i`/`.ii`文件。可以理解为扩展的源代码。
* ***任务***：1）处理头文件包含`#include`；2）处理宏定义`#define`
* ***执行者***：预处理器，一个名为cpp的可执行文件（`cpp.exe`）



### 2. 编译



### 3. 汇编



### 4. 链接



## 二、头文件与库文件

* ***库（library）***

    库是一个已经以前编译好的目标文件（`.o`/`.obj`）的集合，可以和自己编译出来的文件通过链接器（`linker`）进行链接。一个常见的例子就是`printf()`。

    外部库又分为静态库（static library ）和共享库（shared library）。

* ***静态库（static library ）***

    静态库文件在Unix环境下是拥有`.a`扩展名的文件；在Windows环境下是`.lib`的库文件。当程序与静态库链接时，程序中使用的外部函数的机器码被复制到可执行文件中。可以通过归档程序`ar.exe`创建静态库。

* ***共享库（shared library）***

    共享库文件在Unix环境下的扩展名是`.so`（shared objects）；在Windows环境下是`.dll`（dynamic link library）。当程序与动态库链接时，可执行文件中只会创建一个小的表格。在可执行程序开始运行前，操作系统会将需要用到的外部函数的机器码载入，这个过程称为**动态链接（dynamic linking）**。

    动态链接使可执行文件的体积变小，从而节省磁盘空间，原因是一份共享库的拷贝可以被很多程序使用。

    此外，大多数操作系统允许所有正在运行的程序使用内存中共享库的一个副本，从而节省内存。

    由于动态链接，共享库文件更新不需要重新编译程序。

* ***头文件***