---
title: MaximumSubsequenceSum解法
categories: 
- 数据结构
- 算法
tags: 
- C语言
- 算法
---

最大子列和问题的变种，无非就是多加了找出最大子列和的第一个元素和最后一个元素

<!-- more -->

特点是在MaxSum更新的时候通过LastTag存入目前加上的数组元素的下标，从而找到最大子列和最后一个元素的下标，再倒推过来找到最大子列和的第一个元素的下标，存入FirstTag

```
#include <stdio.h>
#include <stdlib.h>

int main()
{
	int N, sum, FirstTag, LastTag, ThisSum, MaxSum, j, flag;
    flag = 0;

	printf("请输入数字个数：");
	scanf("%d", &N);
	int* A = (int*)malloc(sizeof(int)*N);
	printf("请输入%d个数字：", N);
	for(int i = 0; i < N; i++) {
		scanf("%d", &A[i]);
	}
	getchar();

	ThisSum = MaxSum = 0;
	for (j = 0;j < N;j++)
	{
		ThisSum += A[j];
		if (ThisSum > MaxSum) {
            MaxSum = ThisSum;
            LastTag = j;
        } else if (ThisSum < 0)
			ThisSum = 0;
        if(A[j] >= 0) {
            flag = 1;
        }
	}

    sum = 0;
    for(int l = LastTag; l >= 0; l--) {
        sum += A[l];
        if(sum == MaxSum) {
            FirstTag = l;
        }
    }

    if(MaxSum) {
        printf("最大子序列和为%d\n", MaxSum);
    } else if(flag) {
        printf("最大子序列和为0\n");
    }
    printf("最大子列和第一个元素为%d, 最后一个元素为%d\n", A[FirstTag], A[LastTag]);
	getchar();
	return 0;
}
```