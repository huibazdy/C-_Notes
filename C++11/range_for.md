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



> 注意：范围for循环是只读的，除非将变量声明为引用类型

```c++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v(10,0);
    for(auto i : v)     // 尝试用变量名修改数组元素
        i++;
    for(auto i : v)     // 打印数组
        std::cout<<i<<" ";
    std::cout<<std::endl;
    
    
    for(auto& j : v)    // 尝试用引用修改数组元素
        j++;
    for(auto j : v)     // 打印数组
        std::cout<<j<<" ";
    std::cout<<std::endl;
    
    return 0;
}
```

执行结果：

<img src="https://raw.githubusercontent.com/huibazdy/TyporaPicture/main/image-20231228102951295.png" alt="image-20231228102951295" style="zoom:50%;" />

