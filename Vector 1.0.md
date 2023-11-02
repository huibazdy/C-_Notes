# Vector 1.0

> vector是C++顺序容器（Sequence Container）的一种，可以理解为能存放任意类型的动态“数组”。



## 一、插入

| 方法                  | 功能                               |
| --------------------- | ---------------------------------- |
| **`push_back(elem)`** | 向vector尾端添加一个值为elem的元素 |
| **`emplace()`**       |                                    |



## 二、删除

| 方法             | 功能                   |
| ---------------- | ---------------------- |
| **`pop_back()`** | 删除向量的最后一个数据 |
|                  |                        |



## 二、容量

| 方法             | 功能                                   |
| ---------------- | -------------------------------------- |
| **`size()`**     | 返回向量中当前已容纳的元素个数         |
| **`capacity()`** | 返回向量当前能容纳的元素个数           |
| **`max_size()`** | 返回向量这种容器最多可以存储的元素个数 |
| **`empty()`**    | 若向量中不含任何元素返回真，否则返回假 |

* 关于 `size`

    **size** 指的是容器中的元素个数

* 关于 `compacity`

    **capacity** 指的是分配给容器的存储空间，即存储元素的个数上限

    

## 三、访问

| 方法         | 功能                         |
| ------------ | ---------------------------- |
| **`v[n]`**   | 返回v中第n个位置上元素的引用 |
| **`back()`** | 返回向量中最后一个元素的引用 |



## 四、遍历与查找

| 方法     | 功能 |
| -------- | ---- |
| `find()` |      |
