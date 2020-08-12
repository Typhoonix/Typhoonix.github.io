---
title: PTA最大子列和问题
categories: 
- 数据结构
- 算法
tags: 
- C语言
- 算法
---

采用C语言解法，后续可能会补充其他语种的写法

<!-- more -->

## 题目
给定K个整数组成的序列{ N<sub>1</sub>, N<sub>2</sub>, ..., N<sub>K</sub> }，“连续子列”被定义为{ N<sub>i</sub>, N<sub>i+1</sub>, ..., N<sub>j</sub> }，其中 1 ≤ i ≤ j ≤ K。“最大子列和”则被定义为所有连续子列元素的和中最大者。例如给定序列{ -2, 11, -4, 13, -5, -2 }，其连续子列{ 11, -4, 13 }有最大的和20。现要求你编写程序，计算给定整数序列的最大子列和。

本题旨在测试各种不同的算法在各种数据情况下的表现。各组测试数据特点如下：

- 数据1：与样例等价，测试基本正确性;
- 数据2：102个随机整数;
- 数据3：103个随机整数;
- 数据4：104个随机整数;
- 数据5：105个随机整数;

## 输入格式

输入第1行给出正整数K ( ≤100000 )；第2行给出K个整数，其间以空格分隔。

## 输出格式

在一行中输出最大子列和。如果序列中所有整数皆为负数，则输出0。

## 输入样例

```
6
-2 11 -4 13 -5 -2
```

## 输出样例

```
20
```

## C语言解法

遍历整个数组时，ThisSum作为遍历数组的和，MaxSum不断靠ThisSum更新和大小，ThisSum < 0时ThisSum会清零

```
#include <stdio.h>
#define MAXN 100000    //本题最大数据是十万

int MaxSubSequenceSum(const int A[], int N)
{
	int ThisSum, MaxSum, j;
	ThisSum = MaxSum = 0;
	for (j = 0;j < N;j++)
	{
		ThisSum += A[j];
		if (ThisSum > MaxSum)
			MaxSum = ThisSum;
		else if (ThisSum < 0)
			ThisSum = 0;
	}
	return MaxSum;
}

int main(void) {
	int K, i;
	
	scanf("%d", &K);
    int* a = (int*)malloc(sizeof(int)*K);
	for ( i = 0; i < K; i++ )
		scanf("%d", &a[i]);
	printf("%d", MaxSubSequenceSum( a, K ));
	
	return 0;
}
```