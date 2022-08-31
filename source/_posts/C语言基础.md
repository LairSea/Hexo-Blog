---
title: C语言基础
top: false
cover: false
toc: true
mathjax: false
date: 2022-06-30 23:20:21
author:
img:
coverImg:
password:
summary:
tags: 'C语言'
categories: '语言基础'
---
# C/C++基础知识

## 1、数据类型

### 1、1变量

​	数据的容器，数据变容器不变

```c
int a = 1;
```

### 1、2 基本类型

#### 1、2、1 基本类型介绍

整型：int 

字符型：char

float 单精度浮点型

double 双精度浮点型

```c
#include<stdio.h>
int main()
{
	//sizeof 求所占字节数
	printf("%d\n",sizeof(int));
	printf("%d\n",sizeof(char));
	printf("%d\n",sizeof(float));
	printf("%d\n",sizeof(double));
	return 0;
}

输出：
    4
    1
    4
    8
```

#### 1、2、2 限定符

short int : 使得 int 变短 （4 ---> 2）

long int :使得 int 变长 （4---> 8）

```c

#include<stdio.h>
int main()
{
	
	short int a = 1 ;
	long int b = 2;
	printf("%d\n",sizeof(a));
	printf("%d\n",sizeof(b));
    printf("%d\n",a);
	printf("%d\n",b);
	return 0;
}

输出：
    2
    8
    1
    2
```

signed  有符号 

unsigned  无符号

可用于修饰 char 和整型 （包括 short 和 long 修饰过的整型）

```
signed int  等价与 int
unsigned int 使得原有整型长度不变，但是最高位符号位也变成了数据位。仅能表示 0 和正数

signed char 强制 char 可以存储有符号整数
unsigned char 强制 char 可以存储无符号整数
不加限定 则是否有符号依据机器所定
```

#### 1、2、3 常量表示形式

```c
#include<stdio.h>
int main()
{
    //int 型 
    printf("%d\n",sizeof(10));
    // L long 型
    printf("%d\n",sizeof(10.L));
    // f float 型
    printf("%d\n",sizeof(10.f));
    //double
    printf("%d\n",sizeof(10.0));
	return 0;
}
输出：
    4
    16
    4
    8

```

```c
进制表示：
	默认 10 进制
	c = a + 31;
	c = a + 037   // 0 开头表示8进制
	c = a + 0x1f  //0x开头表示16进制
```

```c
'0'
输出 对应字符的 ASCII 码
    
//字符串
“ I AM STRING”
```

### 1、3 数据类型转换

#### 1、3、1 自动转换

```c
#include<stdio.h>
int main()
{
    short int a = 1;
    int b = 1;
    float c = 1.0;
    double d = 1.1;
    printf("%d\n",sizeof(a));
    printf("%d\n",sizeof(a+b));
    printf("%d\n",sizeof(a+c));
    printf("%d\n",sizeof(a+d));
	return 0;
}
```



#### 1、3、2 强制转换

```c
（类型名）表达式;
(float) a;
(int) (c+d);
(float) 5;
```

## 2、运算符

![img](https://gitee.com/ShomeNote/images-bed/raw/master/https://gitee.com/ShomeNote/images-bed/202205062039088.png)

![img](https://gitee.com/ShomeNote/images-bed/raw/master/https://gitee.com/ShomeNote/images-bed/202205062039075.png)

```c

/* 三元运算符实例 */
   a = 10;
   b = (a == 1) ? 20: 30;
   printf( "b 的值是 %d\n", b );
 
   b = (a == 10) ? 20: 30;
   printf( "b 的值是 %d\n", b );
```

## 3、函数

### 3、1 定义

```c
return_type function_name( parameter list );
返回值类型 + 函数名+（函数参数列表）
```

### 3、2 函数递归调用

![img](https://gitee.com/ShomeNote/images-bed/raw/master/https://gitee.com/ShomeNote/images-bed/202205062039981.png)

### 3、3 作用域规则

```c
#include<stdio.h>
函数参数 a 作用域仅限函数内部 所以无法改变外部变量的值
void f(int a){
    //局部变量生命周期只对函数调用这一段有效
    //函数执行完就被回收了
     a = 10;
}
int main()
{
    int a = 0;
    f();
    printf("%d",a);
	return 0;
}

输出：0
```

```c
如果我想通过函数改变外部变量呢？
//方式1：
        #include<stdio.h>
        // c++中将参数定义成引用型
        void f(int &a){
             a = 10;
        }
        int main()
        {
            int a = 0;
            f(a);
            printf("%d",a);
            return 0;
        }
//方式2：c语言中将函数参数定义成指针型，然后将变量地址传入函数中
        #include<stdio.h>
        void f(int *a){
             a = 10;
        }
        int main()
        {
            int a = 0;
            //变量地址传入
            f(&a);
            printf("%d",a);
            return 0;
        }
```

```c
//语句块的概念（局部代码块）：{}括起来的一条或多条语句
int main(){
    {
        int a = 0;
    }
     printf("%d",a);
}

输出： error
//语句块中定义的变量只在语句块内有效
```

### 3、4 静态变量

```c
#include<stdio.h>
void f(){
    int a = 1;
    a++;
    printf("%d\n",a);
}
int main()
{
    for(int i = 0;i < 3; ++i){
        f();
    }
	return 0;
}
输出：
    1
    1
    1
每次调用f()都会重新定义一个新的 a,原来的a被回收
```

```c
//如果想在函数中操作同一个a,使其在函数调用完后不会被回收掉，可以使用静态变量
#include<stdio.h>
void f(){
    static int a = 1;
    a++;
    printf("%d\n",a);
}
int main()
{
    for(int i = 0;i < 3; ++i){
        f();
    }
	return 0;
}
输出：
    2
    3
    4
```

## 4、指针与数组

### 4、1什么是指针？

```c
//指针也就是内存地址，指针变量是用来存放内存地址的变量。
//指针变量声明的一般形式为：
 type *var_name;
```

```c
#include<stdio.h>
void f(){
    static int a = 1;
    ++a;
    printf("%d\n",a);
}
int main()
{
    int a = 1;
    //指针保存的是变量的地址值
    int *a_point = &a;
    printf("%p",a_point);
    //通过指针可以修改它指向变量的值
    //  ++a 先自增再传值 a++ 先传值再自增
    printf("%d",++*a_point);
	return 0;
}
输出：0x7ffe26d3ca44
      2
```

### 4、2野指针

```c
#include <stdio.h>
#include <stdlib.h>
//指针未初始化，它所被分配的地址空间可能随机指向一个变量，如果这个变量很重要，那么不小心对其进行修改会造成严重问题。
//如果没有变量要指向，可以 int *point = NULL; 让它指向一个任何变量都不会被分配到的地址
int main()
{
    int *point;
    printf("%d",point);
    return 0;
}
输出：16(随机数)
```

### 4、3指针和函数参数

![img](https://gitee.com/ShomeNote/images-bed/raw/master/https://gitee.com/ShomeNote/images-bed/202205062039991.png)

### 4、2指针对数组的操作

```c
#include<stdio.h>
void f(){
    static int a = 1;
    ++a;
    printf("%d\n",a);
}
int main()
{
    int a[5] = {1,2,3,4,5};
    //指针指向数组第一个元素的地址值
    int *p = a;
    for(int i = 0;i < 5;++i){
        //通过首地址增加，来顺序访问数组元素
        printf("%d\t",*(p+i));
    }
    
	return 0;
}
```

```c
//字符型数组
char *s = "i am string "; //常量不可修改
char s1[] = "i am string"; //可修改
```

```c
指针数组：
#include<stdio.h>

int main()
{
    int a[5] = {1,2,3,4,5};
    //指针数组中存的都是a中对应元素的地址
    int *p[] = {a,a+1,a+2,a+3,a+4};
    for(int i = 0;i < 5;++i){
        printf("%p\t",p[i]);
        printf("%d\t\n",*p[i]);
    }
    
	return 0;
}
输出结果：
    0x7ffde1f97120	1	
    0x7ffde1f97124	2	
    0x7ffde1f97128	3	
    0x7ffde1f9712c	4	
    0x7ffde1f97130	5	

```

## 5、结构体

```c
typedef :用来给类型取别名
    typedef int verType;
	verType a 等价于 int a
```

```c
//封装平面内的一个点的坐标（x,y）
struct{
	int x;
     int y
}point;
#include<stdio.h>
struct{
    int x;
    int y;
}point;
int main()
{
    point.x = 10;
    point.y = 20;
    printf("%d,%d",point.x,point.y);
	return 0;
}
```

### 5、1 给结构体取别名

```c
typedef struct{
	int x;
     int y
}Point;

#include<stdio.h>
typedef struct{
    int x;
    int y;
}Point;
int main()
{
    Point point;
    point.x = 10;
    point.y = 20;
    printf("%d,%d",point.x,point.y);
	return 0;
}
```

### 5、2 指向结构的指针

```c
#include<stdio.h>
typedef struct{
    int x;
    int y;
}Point;
int main()
{
    Point point;
    Point *p = NULL;
    p = &point;
    //指针指向分量： p->x
    //结构体指向分量 ： p.x
    p->x = 10;
    p->y = 20;
    printf("%d,%d",p->x,p->y);
	return 0;
}

```

### 5、3  自引用结构

```c
//自引用结构的指针

#include<stdio.h>
typedef struct Point{
    int x;
    int y; 
    struct Point *next;
}Point;
int main()
{
    Point p1,p2,p3,p4,p5;
    Point *p = NULL;
    p1.x = 1; p1.y = 2;
    p2.x = 3; p2.y = 4;
    p3.x = 5; p3.y = 6;
    p4.x = 7; p4.y = 8;
    p5.x = 9; p5.y = 10;
    
    p1.next = &p2;
    p2.next = &p3;
    p3.next = &p4;
    p4.next = &p5;
    p5.next = NULL;
    
    
    for (p = &p1; p!=NULL;p = p->next) {
        printf("(%d,%d)\n",p->x,p->y);
    }
   
	return 0;
}

输出：
    (1,2)
    (3,4)
    (5,6)
    (7,8)
    (9,10)
		
```

## 6、类（C++）

```

```

