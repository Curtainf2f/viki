# dynamic_cast

1. 运行时处理，其他都是编译时完成
2. 不能用于内置类型转换

```cpp
    double a = 1.11;
    int b = dynamic_cast<int>(a); //错误：dynamic_cast 中的类型必须是指向完整类类型或 void * 的指针或引用
```

3. 基类必须有虚函数，否则编译不通过

```cpp
class A{
public:
    int v;
    A(int v = 0):v(v){}
    void func(){ cout << "A::func" << endl; }
};

class B:public A{
public:
    B(int v = 0):A(v){}
    void func(){ cout << "B::func" << endl; }
};

int main()
{
    A* pAB = new B();
    B* pB;
    pB = dynamic_cast<B*>(pAB); //错误：运行时 dynamic_cast 的操作数必须包含多态类类型
    return 0;
}
```

4. 转换成功返回指向类的指针或引用，失败时返回NULL

```cpp

class A{
public:
    int v;
    A(int v = 0):v(v){}
    virtual void func(){ cout << "A::func" << endl; }
};

class B:public A{
public:
    B(int v = 0):A(v){}
    void func(){ cout << "B::func" << endl; }
};

int main()
{
    A* pAA = new A();
    A* pAB = new B();
    A* pA;
    B* pB;
    pB = dynamic_cast<B*>(pAA);
    cout << (pB == nullptr) << endl; // 输出 1
    pB = dynamic_cast<B*>(pAB);
    cout << (pB == nullptr) << endl; // 输出 0
    return 0;
}
```

5. 要强制转换的指针所指向的对象实际类型与将要转换后的类型一定要相同，否则转换失败。

```cpp
    A* pAA = new A();
    B* pB;
    pB = dynamic_cast<B*>(pAA); // 失败：pAA指向的是A类型的对象，所以无法转换为B类型的指针
```

6. 比`static_cast`安全

```cpp
    A* pAA = new A();
    B* pB;
    pB = dynamic_cast<B*>(pAA); // 失败：pAA指向的是A类型的对象，所以无法转换为B类型的指针
    pB = static_cast<B*>(pAA); // 成功：pAA指向的是A类型的对象，竟然可以转换为B类型的指针
```