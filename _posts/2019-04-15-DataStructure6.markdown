---
layout: post
title: 数据结构 C语言 哈希
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】  
近日，贵州省第二届程序设计大赛在贵州大学举行，来自省内各高校的学生参赛队伍共n支，为了用事实说明编程能力到底哪家强，现请你根据比赛结果统计出参赛队伍总分最高的那个学校。  
【任务要求】  
【测试数据】  
输入格式：  
输入在第1行给出不超过105的正整数N，即参赛队数。随后N行，每行给出一个参赛队的信息和成绩，包括其所代表的学校的编号（从1开始连续编号）、及其比赛成绩（百分制），中间以空格分隔。  
输出格式：  
在一行中给出总得分最高的学校的编号、及其总分，中间以空格分隔。题目保证答案唯一，没有并列。  

输入样例：  
6  
3 65  
2 80  
1 100  
2 70  
3 40  
3 0  
输出样例：  
2 150  

```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
	int data[105] = { 0 };
	int n, num, score, i, max = 0;
	scanf("%d", &n);
	while (n--) {
		scanf("%d%d", &num, &score);
		data[num] += score;
	}
	for (i = 0; i < 105; i++) {
		if (data[i] > data[max]) {
			max = i;
		}
	}
	printf("%d %d", max, data[max]);
	system("pause");
	return 0;
}


```