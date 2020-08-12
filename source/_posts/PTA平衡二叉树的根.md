---
title: PTA平衡二叉树的根
categories: 
- 数据结构
tags: 
- C语言
- 数据结构
---

涉及到左单旋、右单旋、左右双旋、右左双旋的操作，值得记录

<!-- more -->

## 题目
将给定的一系列数字插入初始为空的AVL树，请你输出最后生成的AVL树的根结点的值。

## 输入格式

输入的第一行给出一个正整数N（≤20），随后一行给出N个不同的整数，其间以空格分隔。

## 输出格式

在一行中输出顺序插入上述整数到一棵初始为空的AVL树后，该树的根结点的值。

## 输入样例1

```
5
88 70 61 96 120
```

## 输出样例1

```
70
```

## 输入样例2

```
7
88 70 61 96 120 90 65
```

## 输出样例2

```
88
```

## C语言解法

```
#include <stdio.h>
#include <stdlib.h>
typedef int ElementType;

typedef struct AVLNode *Position;
typedef Position AVLTree; /* AVL树类型 */
struct AVLNode{
    ElementType Data; /* 结点数据 */
    AVLTree Left;     /* 指向左子树 */
    AVLTree Right;    /* 指向右子树 */
    int Height;       /* 树高 */
};

int Max ( int a, int b )
{
    return a > b ? a : b;
}

int GetHeight( AVLTree A ) {
    if(A == NULL) {
        return 0;
    }
    return A->Height;
}

AVLTree SingleLeftRotation ( AVLTree A )
{ /* 注意：A必须有一个左子结点B */
  /* 将A与B做左单旋，更新A与B的高度，返回新的根结点B */     
 
    AVLTree B = A->Left;
    A->Left = B->Right;
    B->Right = A;
    A->Height = Max( GetHeight(A->Left), GetHeight(A->Right) ) + 1;
    B->Height = Max( GetHeight(B->Left), A->Height ) + 1;
  
    return B;
}

AVLTree SingleRightRotation ( AVLTree A )
{ /* 注意：A必须有一个右子结点B */
  /* 将A与B做右单旋，更新A与B的高度，返回新的根结点B */

    AVLTree B = A->Right;
    A->Right = B->Left;
    B->Left = A;
    A->Height = Max( GetHeight(A->Left), GetHeight(A->Right) ) + 1;
    B->Height = Max( GetHeight(B->Right), A->Height ) + 1;

    return B;
}
 
AVLTree DoubleLeftRightRotation ( AVLTree A )
{ /* 注意：A必须有一个左子结点B，且B必须有一个右子结点C */
  /* 将A、B与C做两次单旋，返回新的根结点C */
     
    /* 将B与C做右单旋，C被返回 */
    A->Left = SingleRightRotation(A->Left);
    /* 将A与C做左单旋，C被返回 */
    return SingleLeftRotation(A);
}

AVLTree DoubleRightLeftRotation ( AVLTree A )
{ /* 注意：A必须有一个左子结点B，且B必须有一个右子结点C */
  /* 将A、B与C做两次单旋，返回新的根结点C */
     
    /* 将B与C做右单旋，C被返回 */
    A->Right = SingleLeftRotation(A->Right);
    /* 将A与C做左单旋，C被返回 */
    return SingleRightRotation(A);
}

AVLTree Insert( AVLTree T, ElementType X )
{ /* 将X插入AVL树T中，并且返回调整后的AVL树 */
    if ( !T ) { /* 若插入空树，则新建包含一个结点的树 */
        T = (AVLTree)malloc(sizeof(struct AVLNode));
        T->Data = X;
        T->Height = 0;
        T->Left = T->Right = NULL;
    } /* if (插入空树) 结束 */
 
    else if ( X < T->Data ) {
        /* 插入T的左子树 */
        T->Left = Insert( T->Left, X);
        /* 如果需要左旋 */
        if ( GetHeight(T->Left)-GetHeight(T->Right) > 1 )
            if ( X < T->Left->Data ) 
               T = SingleLeftRotation(T);      /* 左单旋 */
            else 
               T = DoubleLeftRightRotation(T); /* 左-右双旋 */
    } /* else if (插入左子树) 结束 */
     
    else if ( X > T->Data ) {
        /* 插入T的右子树 */
        T->Right = Insert( T->Right, X );
        /* 如果需要右旋 */
        if ( GetHeight(T->Left)-GetHeight(T->Right) < -1 )
            if ( X > T->Right->Data ) 
               T = SingleRightRotation(T);     /* 右单旋 */
            else 
               T = DoubleRightLeftRotation(T); /* 右-左双旋 */
    } /* else if (插入右子树) 结束 */
 
    /* else X == T->Data，无须插入 */
 
    /* 别忘了更新树高 */
    T->Height = Max( GetHeight(T->Left), GetHeight(T->Right) ) + 1;
     
    return T;
}

int main() {
    AVLTree A = NULL;
    int height, X;
    scanf("%d", &height);
    for(int i = 0; i < height; i++) {
        scanf("%d", &X);
        A = Insert(A, X);
    }
    printf("%d\n", A->Data);
    return 0;
}
```