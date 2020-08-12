---
title: 一元多项式的乘法与加法运算
categories: 
- 数据结构
- 算法
tags: 
- C语言
- 算法
---

C语言数组暴力解法，这比课堂上那个简单粗暴多了，方便记忆

<!-- more -->

## 题目
设计函数分别求两个一元多项式的乘积与和。

## 输入格式

输入分2行，每行分别先给出多项式非零项的个数，再以指数递降方式输入一个多项式非零项系数和指数（绝对值均为不超过1000的整数）。数字间以空格分隔。

## 输出格式

输出分2行，分别以指数递降方式输出乘积多项式以及和多项式非零项的系数和指数。数字间以空格分隔，但结尾不能有多余空格。零多项式应输出 **0 0**。

## 输入样例

```
4 3 4 -5 2  6 1  -2 0
3 5 20  -7 4  3 1
```

## 输出样例

```
15 24 -25 22 30 21 -10 20 -21 8 35 6 -33 5 14 4 -15 3 18 2 -6 1
5 20 -4 4 -5 2 9 1 -2 0
```

## C语言解法

i作为数组1下标，j作为数组2下标，对应多项式幂，数组存的内容就是系数，注意输出格式就行

```
#include <stdio.h>
#include <string.h>

//t1,t2分别为0次输入数据，t3为乘法结果，t4为加法结果
int t1[1005], t2[1005], t3[2005], t4[1005];
#define max 1000

//输出函数，arr为结果数组，range为最大范围。
void show(int *arr, int range)
{
    int i;
    int tag = 0;
    for(i = range; i >= 0; --i)
    {
        if(arr[i] != 0)
        {
            //输出的第一项前面没有空格，通过tag进行判断。
            if(tag)
            {
                printf(" ");
            }
            tag = 1;
            printf("%d %d", arr[i], i);
        }
    }
    //若最终tag为0表示没有输出过，整个结果为0，输出0 0
    if(!tag)
    {
        printf("0 0");
    }
    printf("\n");
}

int main()
{
    int n1, n2, i, j, a, b;
    //数组置0
    memset(t1, 0, sizeof(t1));
    memset(t2, 0, sizeof(t2));
    memset(t3, 0, sizeof(t3));
    memset(t4, 0, sizeof(t4));
    scanf("%d", &n1);
    for(i = 0; i < n1; ++i)
    {
        scanf("%d %d", &a, &b);
        t1[b] = a;
    }
    scanf("%d", &n2);
    for(i = 0; i < n2; ++i)
    {
        scanf("%d %d", &a, &b);
        t2[b] = a;
    }
    //计算乘法
    for(i = 0; i <= max; ++i)
    {
        for(j = 0; j <= max; ++j)
        {
            t3[i + j] += t1[i] * t2[j];
        }
    }
    //计算加法
    for(i = 0; i <= max; ++i)
    {
        t4[i] = t1[i] + t2[i];
    }
    //输出
    show(t3, max * 2);
    show(t4, max);
    return 0;
}
```