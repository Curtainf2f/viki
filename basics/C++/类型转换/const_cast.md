# const_cast

```cpp
class A{
public:
    int v;
    A(int v = 0):v(v){}
};
```

1. 常量指针 被强转为 非常量指针，且仍然指向原来的对象；

```cpp
    const A *pa = new A();
    A *paa = const_cast<A*>(pa);
    // pa->v = 100; // 报错 只读类型不可修改
    paa->v = 100;
    cout << pa->v << endl; // 输出 100
```

2. 常量引用 被强转为 非常量引用，且仍然指向原来的对象；

```cpp
    const A a;
    A b = const_cast<A&>(a);
    b.v = 111;
    cout << a.v << endl; // 0
    cout << b.v << endl; // 111
    A &c = const_cast<A&>(a);
    c.v = 222;
    cout << a.v << endl; // 222
```