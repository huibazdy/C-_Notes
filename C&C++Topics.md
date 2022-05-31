# C/C++技术专题

## 函数指针

> 函数指针最常见的两个用途是：1）**转换表**（jump table）；2）作为**函数参数**传递给另个一个函数



```C
int func(int);//func函数的原型
int (*pf)(int) = &func;  //pf现在指向存储函数func代码的首地址
```



> * 函数指针初始化之前具有函数原型非常重要，否则编译器无法检查func的类型是否与pf所指向的类型一致
> * 初始化表达式中的**`&`**是可选的，因为函数名被使用时总由编译器将它转换为函数指针，`&`只是显式说明



```C
//函数指针被声明并初始化后，可以有三种方式调用该函数
int ans;
ans = func(10);   //函数名被编译器转换为一个函数指针，该指针指向函数代码在内存中的首地址，调用该函数就是执行起始于这个地址的代码
ans = (*pf)(10);  //先把函数指针转换为函数名，再由编译器转换为函数指针
ans = pf(10);
```



> #### 回调函数

回调函数是函数指针的重要应用。以下是回调函数使用的几个例子：

* C标准库中的二分查找**`bsearch`**函数

    参考源码地址：1. [FreeBSD libc](https://svnweb.freebsd.org/base/head/lib/libc/) 2.[NetBSD libc](http://cvsweb.netbsd.org/bsdweb.cgi/src/lib/libc/?only_with_tag=MAIN) 3.[GNU libc](http://www.gnu.org/software/libc/sources.html)

    ```C
    //NetBSD Version
    #include<sys/c>
    #include<assert.h>
    #include<errno.h>
    #include<stdlib.h>
    void * bsearch(const void *key,
                   const void *base0,
                   size_t nmemb,
                   size_t size,
                   int (*compar)(const void *,const void *))
    {
        const char *base = base0;
        size_t lim;
        int cmp;
        const void *p;
        
        _DIAGASSERT(key != NULL);
        _DIAGASSERT(base0 != NULL || nmemb == 0);
        _DIAGASSERT(compar != NULL);
        
        for(lim = nmemb; lim != 0; lim >>= 1){
            p = base + (lim >> 1) * size;
            cmp = (*compar)(key,p);
            if(com == 0)
                return _UNCONST(p);
            if(cmp > 0){  /*key > p: move right*/
                base = (const char *)p + size;
                lim--;
            }  /*else move left*/
        }
        return (NULL);
    }
    ```

    