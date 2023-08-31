

> vector 之——size 与 capacity



### size

size 指的是 vector 中存储对象的

* 获取 size： `size()`
* 修改 size：`resize()`
    1. `resize(n)`
    2. `resize(n,value)`

> 需要注意的是，当 resize 改小不会修改原来的 capacity，但是当 resize 改大，且 n 大于之前的 capacity 则会间接修改 capacity 的值为 n。



### capacity

capacity 是 vector 存储对象的容量，可以理解为容纳能力，也就是存储对象的数量上限。

* 获取 capacity：`capacity()`
* 直接修改 capacity：`reserve()`
* 间接修改 capacity：`resize()`





> push_back( ) 与 reserve( ) 的区别

首先需要明确一点，当vector元素增长到capacity后，使用push_back( ) 来继续添加元素，会按之前容量（旧的capacity）的倍数来分配内存给vector，而不是只分配一个新增元素的位置。

例如以下程序：

```c++
...
int main()
{
    vector<int> v{1,2,3,4};
    cout<<v.size()<<" "<<v.capacity()<<endl;

    v.push_back(5);
    cout<<v.size()<<" "<<v.capacity()<<endl;
    ...
}
```

输出分别是：4 4 和 5 8 

可见实际元素只增加了一个，但是 capacity 增加到了 8 。



[**reserve( )**](https://cplusplus.com/reference/vector/vector/reserve/) 只保证修改的新 capacity 大于等于旧的 capacity 即可，可以更加灵活地进行 vector 内存增长，例如上述例子中，我们可以先使用 reserve(5) 将 v 的容量从 4 增加到 5，然后再进行 push_back( ) 操作就不会有内存浪费的情况产生。

```c++
...
int main()
{
    vector<int> v{1,2,3,4};
    cout<<v.size()<<" "<<v.capacity()<<endl;

    v.reserve(5);
    v.push_back(5);
    cout<<v.size()<<" "<<v.capacity()<<endl;
    ...
}
```

输出分别是：4 4 和 5 5

【**注意**】使用 reserve 增长后并不会像 resize 那样默认初始化新元素，知识改变了容量，对存储对象不会改变。



> 释放容器中的多余内存

有一种常见情况是 vector 的 capacity 大于实际 size，此时可以考虑使用 [shrink_to_fit()](https://cplusplus.com/reference/vector/vector/shrink_to_fit/) 函数来减少 capacity 使其和 size 相同，从而达到释放多于内存的目的。

```c++
...
int main()
{
    vector<int> v{1,2,3,4};
    cout<<v.size()<<" "<<v.capacity()<<endl;  //输出：4 4

    v.resize(2);
    cout<<v.size()<<" "<<v.capacity()<<endl;  //输出：2 4
    
    //此时 capacity 为 4，但 size 为 2，需要优化内存
	v.shrink_to_fit();
    cout<<v.size()<<" "<<v.capacity()<<endl;  //输出：2 2
    ...
}
```

当 size 减小到 2 后，使用 shrink_to_fit() 使 capacity 也减小到 2。