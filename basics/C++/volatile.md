# volatile

+ volatile 关键字声明的变量，每次访问时都必须从内存中取出值（没有被 volatile 修饰的变量，可能由于编译器的优化，从 CPU 寄存器中取值）
+ volatile 关键字是一种类型修饰符，用它声明的类型变量表示可以被某些编译器未知的因素（操作系统、硬件、其它线程等）更改。所以使用 volatile 告诉编译器不应对这样的对象进行优化。
+ const 可以是 volatile （如只读的状态寄存器）
+ 指针可以是 volatile

+ 循环
```cpp
int  *output = (unsigned  int *)0xff800000; //定义一个IO端口；  
int   init(void)  
{  
    int i;  
    for(i=0;i< 10;i++)
    {  
    *output = i;  
    }  
}
```

经过编译器优化后

```cpp
int  init(void)  
{  
    *output = 9;  
}
```

+ 多线程

```cpp
volatile  bool bStop=false;  //bStop 为共享全局变量  
//第一个线程
void threadFunc1()
{
    ...
    while(!bStop){...}
}
//第二个线程终止上面的线程循环
void threadFunc2()
{
    ...
    bStop = true;
}
```

如果没用 `volatile` ， Func1使用的是寄存器中的bStop副本，导致Func2修改不到寄存器中的bStop，使Func1一直处于死循环状态

# 常见问题

1. 一个参数可以既是`const` 又是 `volatile` 吗
> 可以
2. 一个指针可以是`volatile`吗
> 可以， 例：当中断程序修改一个指向buffer的指针时
3. 下面这段代码有什么错误
```cpp
int square(volatile int *ptr) 
{ 
return *ptr * *ptr; 
} 
```
编译器将产生类似下面的代码
```cpp
int square(volatile int *ptr) 
{ 
int a,b; 
a = *ptr; 
b = *ptr; 
return a * b; 
} 
```
当执行到a=*ptr 后 ，可能 ptr会产生意想不到的改变，导致 a != b;