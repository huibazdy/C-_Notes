# variadic templates

模板参数可以不只一个，可以是一包

```c++
template <typename T,typename... Types>
...
```

意味着传进模板的参数划分为一个和一包（`pack`）



参考资料：

侯捷 14 集