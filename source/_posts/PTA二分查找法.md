---
title: PTA二分查找法
categories: 
- 数据结构
- 算法
tags: 
- C语言
- 算法
---

挺早学但好像挺重要的，姑且记录一下，还是C语言解法

<!-- more -->

本题要求实现二分查找算法。

## 函数接口定义

```
Position BinarySearch( List L, ElementType X );
```

其中 **List** 结构定义如下

```
typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};
```

**L** 是用户传入的一个线性表，其中 **ElementType** 元素可以通过>、==、<进行比较，并且题目保证传入的数据是递增有序的。函数 **BinarySearch** 要查找 **X** 在 **Data** 中的位置，即数组下标（注意：元素从下标1开始存储）。找到则返回下标，否则返回一个特殊的失败标记 **NotFound**

## 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10
#define NotFound 0
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标1开始存储 */
Position BinarySearch( List L, ElementType X );

int main()
{
    List L;
    ElementType X;
    Position P;

    L = ReadInput();
    scanf("%d", &X);
    P = BinarySearch( L, X );
    printf("%d\n", P);

    return 0;
}

/* 你的代码将被嵌在这里 */
```

## C语言解法

注意一下数据格式就行

```
Position BinarySearch( List L, ElementType X ) {
    int low, high, mid;
    low = 0;
    high = L->Last;
    while (low <= high) {
        mid = (low + high) / 2;
        if(X == L->Data[mid]) {
            return mid;
        }
        else if(X < L->Data[mid]) {
            high = mid - 1;
        }else{
            low = mid + 1;
        }
    }
    return NotFound;
}
```