# extern关键字

## C

### extern 变量

> ***情况一：同一源文件中***

我们知道，全局变量是定义在所有函数之外的变量，且其作用范围是从其定义的位置开始到源文件结束。但一种现实情况是，不是所有的全局变量都定义在源文件开头。如果在定义该全局变量之前的函数想要访问或者说引用该全局变量该如何做？

做法就是在引用之前加上`extern`关键字。

【例】

```c
#include<stdio.h>
int max(int x, int y);
int main()
{
    int ans;
    //引用定义在该函数之后的全局变量
    extern int global_x; 
    extern int global_y;
    ans = max(global_x,global_y);
    printf("The maximum number is: %d\n",ans);
    return 0;
}
//定义全局变量
int global_x = 1;
int global_y = 2;
int max(int x, int y)
{
    return x > y ? x : y;
}
```

> ***情况二：跨源文件引用变量***

extern 更常见的用途在跨源文件引用中。一个变量定义在 A 文件中，源文件 B 想要引用这个变量就需要在引用前加上 extern 关键字。

【**例**】

在源文件 **func.c** 中定义了变量 **val** ：

```c
/* func.c */
int val;
...
```

如果想要在另一个源文件 **main.c** 中引用 **val** ，需要加上 **extern** 关键字

```c
/* main.c */
#include<stdio.h>

int main()
{
    extern int val;   //如果不想extern变量被修改，可以加上const关键字：extern const int val;
    val = 1;          //修改extern变量
    printf("%d\n",val);
    return 0;
}
```

> ***注意***

1.  声明 **extern** 变量只需要类型、变量名；
2. 初始化需要在初次定义的源文件中进行；
3. 引用声明后，可以对 **extern** 变量进行修改（多个源文件引用不建议修改）
4. 若不想 **extern** 变量被修改，可以加上 **const** 关键字限定

> ***区别于 include***

如果使用 **include** 来直接包含 A，也可以使文件 B 能使用 A 中定义的变量，但这样做的结果是使 A 中所有变量与方法都可以被 B 使用，这会导致安全问题。故如果只使用 A 中个别变量或函数，还是通过 **extern** 方式逐条声明更合适。

### extern 函数

类似于 extern 变量，除了可以引用另一个源文件中定义的变量外，还可以使用另一个文件中定义的函数。只需要用 extern 关键字来声明一下外部函数，再使用即可。

【**例**】

取较大值的函数定义在 max.c 文件中

```c
/* max.c */
int max(int x, int y)
{
    return x > y ? x : y;
}
```

在 main.c 文件中使用 max( ) 函数

```c
/* main.c */
int main()
{
    int m = 1;
    int n = 2;
    int ans;
    extern int max(int x, int y); //需要写上完整的函数原型
    ans = max(m,n);
    printf("The maximun number is: %d\n",ans);
    return 0;
}
```



## C++

- [x] to do

[参考资料](https://www.cnblogs.com/honernan/p/13431431.html)

## extern "C"

- [x] to do