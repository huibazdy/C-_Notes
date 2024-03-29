# 对象模型

子类对象有父类的成分。

父类的析构函数（dtor）必须是虚函数，否则会出现 `undefined behavior`



## 继承

> **构造顺序**

子类的构造函数先调用父类的默认构造函数，再执行自己的构造函数

```c++
Derived::Derived():Base() {...};
```

在执行`Derived::Derived()`函数体之前，先执行`Base()`，这部分是编译器加上的



> **析构顺序**

子类的析构函数先执行自己的析构函数，再调用父类的析构函数

```c++
Derived::~Derived(...) {...~Base()};
```



## 复合（composition）

> **构造顺序**

`Container` 的构造函数先调用 `Component` 的默认构造函数，再执行自己的部分

```c++
Container::Container():Component() {...};
```



> **析构顺序**

`Container` 的析构函数先执行自己的析构部分，最后再调用`Component` 的析构函数

```c++
Container::~Container() {...~Component()};
```



## 继承+复合

> **构造顺序**

```c++
Derived::~Derived():Base(),Component() {...};
```



> **析构顺序**

```c++
Derived::~Derived() {...~Component(),~Base()};
```