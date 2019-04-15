---
layout: post
title: 数据结构 C语言 双向链栈 迷宫问题
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】  
以一个mXn的矩阵表示迷宫，0和1分别表示迷宫中的通路和障碍。设计一个程序，对任意设定的迷宫，求出一条从入口到出口的通路，或得出没有通路的结论。  
【任务要求】  
实现链栈求解迷宫从入口到出口的一条可行通路。  
【测试数据】  
迷宫的测试数据如下：左上角（0，1）为入口，右下角（8，9）为出口。  
![矩阵模拟地图](/assets/images/DataStructure2Img1.png)
![迷宫地图](/assets/images/DataStructure2Img2.png)

```c
#include<stdio.h>
#include<stdlib.h>

int count = 0;

typedef struct {
	int x, y;
}position;

typedef struct node{
	position data;
	struct node *next, *pre;
}linkNode;

typedef struct {
	linkNode *head;
	linkNode *top;
}linkStack;

int initStack(linkStack *s)
{
	s->head = (linkNode*)malloc(sizeof(linkNode));
	s->top = s->head;
	s->top->next = NULL;
	s->top->pre = NULL;
}

int pushStack(linkStack *s, position value)
{
	linkNode *p;
	p = (linkNode*)malloc(sizeof(linkNode));
	s->top->data = value;
	p->next = s->top->next;
	p->pre = s->top;
	s->top->next = p;
	s->top = p;
	count++;
	return 0;
}

int popStack(linkStack *s, position *nowPos)
{
	s->top = s->top->pre;
	free(s->top->next);
	count--;
	*nowPos = s->top->pre->data;
}

int main()
{
	linkStack s;
	linkNode *p;
	position nowPos, startPos, endPos;
	int i, m, n, j;
	int **map;

	initStack(&s);
	printf("请输入行列数\n");
	scanf("%d%d", &m, &n);
	map = (int**)malloc(sizeof(int)*m);
	for (i = 0; i < m; i++) {
		map[i] = (int*)malloc(sizeof(int)*n);
	}
	printf("请输入地图:\n");
	for (i = 0; i < m; i++) {
		for (j = 0; j < n; j++) {
			scanf("%d", &map[i][j]);
		}
	}
	printf("\n");
	printf("请输入开始位置和结束位置:\n");
	scanf("%d %d", &startPos.x, &startPos.y);
	scanf("%d %d", &endPos.x, &endPos.y);
	nowPos = startPos;
	pushStack(&s, nowPos);
	while ((nowPos.x != endPos.x) || (nowPos.y != endPos.y)) {
		if (0 == map[nowPos.x + 1][nowPos.y]) {
			nowPos.x += 1;
			map[nowPos.x][nowPos.y] = 2;
			pushStack(&s, nowPos);
		}
		else if (0 == map[nowPos.x][nowPos.y - 1]) {
			nowPos.y -= 1;
			map[nowPos.x][nowPos.y] = 2;
			pushStack(&s, nowPos);
		}
		else if (0 == map[nowPos.x - 1][nowPos.y]) {
			nowPos.x -= 1;
			map[nowPos.x][nowPos.y] = 2;
			pushStack(&s, nowPos);
		}
		else if (0 == map[nowPos.x][nowPos.y + 1]) {
			nowPos.y += 1;
			map[nowPos.x][nowPos.y] = 2;
			pushStack(&s, nowPos);
		}
		else {
			map[nowPos.x][nowPos.y] = 2;
			popStack(&s, &nowPos);
		}
	}
	printf("路径为:\n");
	for (i = 0, p = s.head; i < count; i++,p = p->next) {
		printf("%d,%d\n", p->data.x, p->data.y);
	}
	system("pause");
	return 0;
}
```