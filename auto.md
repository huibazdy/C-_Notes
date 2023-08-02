# auto

在 type 不明确或者很复杂的时候使用 auto 来自动推导类型

```c++
list<string> c;
...
list<string>::iterator ite;
ite = find(c.begin(),c.end(),target);
```

在上述代码的第三行，可以看到迭代器 `ite` 的类型太长了，如果不想写，可以使用 `auto`

```c++
list<string> c;
...
auto ite = find(c.begin(),c.end(),target);
```



有的情况会出现类型过于复杂写不出，如 lamda 表达式涉及到的类型，也可使用 auto