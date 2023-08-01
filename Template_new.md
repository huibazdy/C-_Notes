> **类模板**

```c++
//类模板 Complex
template <typename T>
class Complex
{
public:
    Complex(T re,T im):re(r),im(i){}
    Complex& operator+=(const Complex&);
    T real() const {return re;}
    T imag() const {return im;}
private:
    T re,im;
};
...
// 使用类模板实例化   
Complex<int> c1(2,3);
Complex<double> c2(1.5,4.2);
```



> **函数模板**

不同于类模板，函数模板在使用时，不必指明 `typename` 是什么，由编译器根据实参自动推导。

```c++
// 模板函数 min
template <typename T>
inline const T& min(const T& a,const T& b) {
    return b < a ? b : a;
}
```

定义一个类来举例使用模板函数：

```c++
class Stone
{
public:
    Stone(int w,int,h,int we)
        :_w(w),_h(h),_weight(we){}
    bool operator<(const Stone& rhs) const
    {return _weight < rhs._weight;}
private:
    int _w,_h,_weight;
};
```

使用模板函数：

```c++
Stone r1(1,2),r2(2,2),r3;
r3 = min(r1,r2);
```

