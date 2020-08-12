---
title: PTA两个有序链表序列的合并
categories: 
- 数据结构
- 算法
tags: 
- C语言
- 算法
---

考察链表基本操作，C语言解法

<!-- more -->

本题要求实现一个函数，将两个链表表示的递增整数序列合并为一个非递减的整数序列。

## 函数接口定义

```
List Merge( List L1, List L2 );
```

其中 **List** 结构定义如下

```
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data; /* 存储结点数据 */
    PtrToNode   Next; /* 指向下一个结点的指针 */
};
typedef PtrToNode List; /* 定义单链表类型 */
```

**L1** 和 **L2** 是给定的带头结点的单链表，其结点存储的数据是递增有序的；函数 **Merge** 要将 **L1** 和 **L2** 合并为一个非递减的整数序列。应直接使用原序列中的结点，返回归并后的带头结点的链表头指针。

## 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data;
    PtrToNode   Next;
};
typedef PtrToNode List;

List Read(); /* 细节在此不表 */
void Print( List L ); /* 细节在此不表；空链表将输出NULL */

List Merge( List L1, List L2 );

int main()
{
    List L1, L2, L;
    L1 = Read();
    L2 = Read();
    L = Merge(L1, L2);
    Print(L);
    Print(L1);
    Print(L2);
    return 0;
}

/* 你的代码将被嵌在这里 */
```

## C语言解法

pa遍历L1, pb遍历L2, pc作为桥梁构建合链L

```
List Merge( List L1, List L2 ) {
    List pa, pb, pc, L;
    L = (List)malloc(sizeof(struct Node));
    pa = L1->Next;
    pb = L2->Next;
    pc = L;
    while(pa && pb) {
        if (pa->Data <= pb->Data) {
            pc->Next = pa;
            pc = pa;
            pa = pa->Next;
        } else {
            pc->Next = pb;
            pc = pb;
            pb = pb->Next;
        }
    }
    pc->Next = pa ? pa : pb;
    L1->Next = NULL;
    L2->Next = NULL;
    return L;
}
```