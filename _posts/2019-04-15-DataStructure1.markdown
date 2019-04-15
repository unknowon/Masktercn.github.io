---
layout: post
title: 数据结构 约瑟夫问题
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
一、问题描述：约瑟夫问题  
一个旅行社要从n个旅客中选出一名旅客，为他提供免费的环球旅行服务。旅行社安排这些旅客围成一个圆圈，从帽子中取出一张纸条，用上面写的正整数m作为报数值。游戏进行时，从第s个人开始按顺时针方向自1开始顺序报数，报到m时停止报数，报m的人被淘汰出列，然后从他顺时针方向上的下一个人开始重新报数，如此下去，直到圆圈中只剩下一个人，这个最后的幸存者就是游戏的胜利者，将得到免费旅行的奖励。其中数据结构采用单循环链表。  
二、任务要求  
用单循环链表解决约瑟夫问题。  
三、测试数据  
输入说明，输入的第一行表示总的旅客数,输入的第二行表示报数值，输入的第三行表示报数人的起始编号。  
输入样例：  
9  
5  
1  
输出说明，输出这n个人的淘汰顺序  
输出样例：  
5  
1  
7  
4  
3  
6  
9  
2  
8  

```c
#include<stdio.h>
#include<stdlib.h>
typedef struct node {
	int num;
	struct node *next;
}linkList;

int count = 0;

int create(linkList *head, int n)
{
	int i;
	int index = 2;
	linkList *p, *s;
	p = head;
	for (i = 1; i < n; i++) {
		s = (linkList*)malloc(sizeof(linkList));
		s->num = index;
		s->next = p->next;
		p->next = s;
		p = s;
		index++;
		count++;

	}
	return 0;
}

int func(linkList *head, int startNum, int circData)
{
	linkList *p;
	int i;
	p = head;
	for (i = 1; i < startNum; i++) {
		p = p->next;
	}
	while (count >= 1) {
		for (i = 0; i < circData - 2; i++) {
			p = p->next;
		}
		printf("%d\n", p->next->num);
		p->next = p->next->next;
		p = p->next;
		count--;
	}
	return 0;
}

int main()
{
	linkList *head, *p;
	int i, n, circData, startNum;
	head = (linkList*)malloc(sizeof(linkList));
	head->next = head;
	count++;
	head->num = 1;
	scanf("%d", &n);
	create(head, n);
	p = head;
	scanf("%d", &circData);
	scanf("%d", &startNum);
	func(head, startNum, circData);
	system("pause");
	return 0;
}

```