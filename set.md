# set

**`set`** 是两种关联容器的一种。不同于 **`map`** 中的元素是键值对（**key-value**），其中关键字作为索引；**set** 中的元素仅仅是关键字（**key**）。

**set** 支持高效的关键字查询操作用于检查给定关键字是否在 **set** 中，底层实现基于**红黑树**。



set 可以从两个维度来进行分类：

* 元素（关键字）**是否重复**——包含在头文件 *`<set>`*中
    1. **`set`**
    2. **`multiset`**
* 元素（关键字）**是否无序**——包含在头文件 *`<unordered_set>`*中
    1. **`unordered_set`**
    2. **`unordered_multiset`**



## 使用 set

### 创建 set

```c++
#include <set>
...
// 1.创建空容器，之后再逐个添加元素
std::set<string> shapes;

// 2.创建的同时初始化    
std::set<string> animals = {"tiger","snake","sheep",
                            "monkey","dog","pig"};

// 3.创建的同时拷贝一个已有set（animals）来初始化新建set（pets）
std::set<string> pets(animals);
...
```



### 常见操作

> **添加元素：`insert()`**

```c++
#include <set>
std::set<int> val = {3,0,1};   // 打印会自动排序为：{0,1,3}
s.insert(2);                   // {0,1,2,3}
s.insert(5);                   // {0,1,2,3,5}
```



> **删除元素：`erase()`**

```c++
s.erase(3);                    // {0,1,2,5}
```



> **查找元素：`find()`**

```c++
auto i = s.find(1);
if(i == s.end())
    s.erase(i);    //todo：如何利用迭代器删除元素？？
```

