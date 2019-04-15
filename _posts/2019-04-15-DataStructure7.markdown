---
layout: post
title: 数据结构 C语言 哈希 链地址法
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】  
      为了美丽的校园计划，学校决定改进排队制度，比如说给饭卡充钱等……  
给每个人一个RP值，这个RP值决定这个人来了之后要排的位置，如果当前位置已经有人，  
那么从这个位置以后的人后移一位，这个人插进去，如果没有人的话就直接排到这个位置上去。  
现在已知按时间从前到后来的人的名字和RP值，求按排队顺序输出排队人的名字。  
【任务要求】    
任务要求：用链地址法解决冲突的方式建立哈希表，将RP值相同的元素利用头插法插入到同一个单链表中，并依次输出哈希表中的每一个链表。如：  
 
【测试数据】  
    输入  
多组测试数据，以文件结束  
每组首先一个n(1<=n<=200000),接着一行是名字和RP值，名字是不超过20的字符串  
RP值不超过int  
输出  
按排队顺序输出排队人的名字  
样例输入：  
4  
Yougth 1  
yuanhang 2  
kaichuang 2  
yaoyao 1  
样例输出  
yaoyao Yougth kaichuang yuanhang  
  
//代码有点缺陷，稍后修改  

```c
#include<stdio.h>
#include<stdlib.h>

typedef struct node{
	char name[20];
	struct node *next;
}nextNode;

typedef struct {
	nextNode *col;
}list;

list *createHash()
{
	list ls[1000];
	int i, n, rp;
	nextNode *s;
	for (i = 0; i < 1000; i++) {
		ls[i].col = (nextNode*)malloc(sizeof(nextNode));
		ls[i].col->next = NULL;
	}
	scanf("%d", &n);
	for (i = 0; i < n; i++) {
		s = (nextNode*)malloc(sizeof(nextNode));
		scanf("%s", s->name);
		scanf("%d", &rp);
		s->next = ls[rp].col->next;
		ls[rp].col->next = s;
	}
	return ls;
}

int display(list *ls)
{
	int i;
	nextNode *p;
	for (i = 0; i < 200; i++) {
		p = ls[i].col;
		while (p->next != NULL) {
			p = p->next;
			printf("%s ", p->name);
		}
	}
	return 0;
}

int main()
{
	list *ls;
	ls = createHash();
	display(ls);
	printf("\n");
	system("pause");
	return 0;
}
```