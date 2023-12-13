# Function Pointer

> 函数的二进制代码存放在内存中的代码段（.text），函数的地址是它的二进制代码在内存中的起始地址。如果把函数地址作为参数传递给另一个函数，那么就可以在函数中灵活调用其他函数。
>
> 函数指针的主要应用在于函数的回调。

## 声明

声明一个函数指针，类似于声明普通指针，需要指定指针的类型。这里的函数指针的类型指**函数类型**，即函数返回类型以及函数参数类型。例如：

```c++
int Func1(int n1, string str1);
int Func2(int n2, string str2);
bool Func3(int n3, string str3);
bool Func4(int n4);
```

上述四个函数中，只有函数 1 和 2 是相同类型。



函数指针的声明需要注意：

* 函数返回类型
* 函数指针名
* 函数参数类型

例如：

```c++
int (*ptrFunc1)(int, string);
bool (*ptrFunc2)(int, string);
```

注意：可以写变量名，也可以省略。



## 指向

在 C++ 中，函数名就是函数在内存中的地址。



## 调用

利用函数指针调用函数的语法是：`ptr_name(args...);`



```c++
#include <iostream>

int MyMax(int n, int m) {
    return n > m ? n : m;
}

int main()
{
    int n,m;
    std::cin>>n>>m;
 	int (*ptrf)(int, int);            // 1. 声明函数指针
    ptrf = MyMax;                     // 2. 指向具体函数
    std::cout<<ptrf(n,m)<<std::endl;  // 3. 利用函数指针调用函数
    std::cout<<(*ptrf)(n,m)<<std::endl;  // C 风格调用
	return 0;    
}
```



C 语言与 C++ 的调用写法不太一样：`(*ptrf)(n,m)` ，需要额外对函数指针名加一个星号和一个括号。





> 为什么要引入函数指针？直接通过函数名调用不就好了吗？

以上例子中，引入函数指针确实没有必要。

真正的应用场景是函数的回调。

假设我们有一个完整的工作流程 A ，它由许多步骤组成，但是里面有一部分是客制化的，作为乙方我们为甲方提供服务，这部分流程我们暂时无法决定，需要留给甲方自己指定：

```c++
void FuncA()
{
    // 固定流程一
    // 客制化流程，需要由用户指定，我们写 FunA 时无法写死
    // 固定流程二
}
```

那么这部分暂时无法确定的，由之后函数 FuncA 的使用者来决定如何执行的流程是否可以留一个“参数”在调用 FuncA 时由传入的实参决定呢？

答案是肯定的。如何将一个流程写成参数形式呢？就是利用函数指针，这里的函数指的就是客制化流程 `ClientProcess` 。将其作为参数传入函数 FuncA 就是在 FuncA 中设置一个函数指针形参。

这种调用方法就称为函数的回调，个性化流程 `ClientProcess` 就称为**回调函数**。

```c++
void FuncA(void (*ptrf)())
{
    std::cout<<"Fixed Process 1"<<std::endl;   // 固定流程一
    ptrf();     // 回调
    std::cout<<"Fixed Process 2"<<std::endl;   // 固定流程二
}
```



* 无论回调函数是谁写的，返回值和参数列表要相同（即：函数类型相同），个性化的只是函数中的代码



再来看一个更完整的例子：

```c++
void Client1() {
    std::cout<<"I am Lucy!"<<std::endl;
}

void Client2() {
    std::cout<<"I am Kevin!"<<std::endl;
}

void Service(void (*ptrf)()) {
    std::cout<<"Fixed process 1 of Service"<<std::endl;
    ptrf();
    std::cout<<"Fixed process 2 of Service"<<std::endl;
}

int main()
{
    void (*ptrf)();
    ptrf = Client1;   
    Service(ptrf);    
    Service(Client2);   // 也可以直接三行合为一行： Service(Client1);
}
```



如果回调函数有参数，可以将其参数写入调用者函数的参数列表：

```c++
void Client1(int n) {
    std::cout<<"I'm Lucy! My age is: "<<n<<std::endl;
}

void Client2(int n) {
    std::cout<<"I'm Kevin! My age is: "<<n<<std::endl;
}

void Service(void (*ptrf)(int), int n) {
    std::cout<<"Fixed process 1 of Service"<<std::endl;
    ptrf(n);
    std::cout<<"Fixed process 2 of Service"<<std::endl;
}

int main()
{
    Service(Client1,12);
    Service(Client2,14);
    return 0;
}
```









> 小结

我们在写调用者函数（上例中的 `Service` ）的功能时，只关心（或者说约定）回调函数的类型而不关心回调函数的功能。