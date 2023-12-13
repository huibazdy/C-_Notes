# range-based for(C++11)

```c++
for(decl : coll) {
    statement
}
```

* `decl`：变量
* `coll`：容器或某些集合

上述语句表示会用容器中的每一个元素来执行循环体内的语句。例如：

```c++
for(int i : {1,2,3}) {
    cout<< i << endl;
}
```



```c++
vector<double> vec;
...
for(auto elem : vec) {    //传值：将 vec 中的元素挨个复制到 elem 中进行打印
    cout<< elem << endl;
}

for(auto& elem : vec) {   //传引用：如果想改变 vec 中的元素值（乘3）就不能传值，必须要传引用
    elem *= 3;
}
```