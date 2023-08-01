

```c++
//类模板 Complex
template<typename T>
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

