# Lambda

> 只使用一次的函数对象（或者说匿名函数），直接在使用它的地方进行定义，达到减少程序中函数对象数量的目的。





> lambda 表达式是一个可调用的代码单元，可以将其理解为一个未命名的内联函数。



与普通函数不同，lambda 表达式可以定义在函数内部。



形式如下：

`[capture list] (parameter list) -> return type { function body }`

1. capture list（捕获列表）：**必须**
2. parameter list（参数列表）：不必须
3. return type（返回类型）：不必须
4. function body（函数体）：**必须**

其中 2/3/4 与普通函数相同，但 lambda 必须使用***尾置返回***。



```c++
auto f = [] {return 42;};
```

定义了一个可调用的对象 f ，它不接受任何参数，返回 42 。



lambda 的调用方式与普通函数相同，都使用调用运算符`()`

```c++
std::cout<<f()<<std::endl;  // 打印 42
```

> 如果未指定返回类型，lambda 会根据函数体中的代码推断出返回类型。如果函数体只是一个 return 语句，返回类型根据返回的表达式类型推断，都则返回类型为 void。







带参数的 lambda 表达式：

```c++
[](const string& a, const string& b)
{return a.size() < b.size();}
```

> 【***空捕获列表***】
>
> 表示 lambda 表达式不使用它**所在函数**中的任何局部变量