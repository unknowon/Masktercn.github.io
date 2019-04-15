---
layout: post
title: 数据结构 C语言 队列 迷宫问题
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】  
以一个mXn的长方阵表示迷宫，0和1分别表示迷宫中的通路和障碍。设计一个程序，对任意设定的迷宫，求出一条从入口到出口的通路，或得出没有通路的结论。  
【任务要求】  
实现队列求解迷宫从入口到出口的最短通路。  
【测试数据】  
迷宫的测试数据如下：左上角（0，1）为入口，右下角（8，9）为出口。  

![迷宫地图](/assets/images/DataStructure2Img1.png)

![矩阵表示](/assets/images/DataStructure2Img2.png)

```c
#include<Stdio.h>
#include<stdlib.h>
#include<Windows.h>

typedef struct {
	int x;
	int y;
	int ex;
}Pos;

typedef struct {
	Pos *data;
	int front;
	int rear;
}Queue;

int main()
{
	int k;
	Queue queue;
	Pos startPos, endPos, nowPos, temPos, temp;
	int **map, m, n;
	int i, j;
	printf("输入行列数:\n");
	scanf("%d%d", &m, &n);
	queue.data = (Pos*)malloc(sizeof(Pos)*m*n);
	map = (int**)malloc(sizeof(int) * m+2);
	for (i = 0; i < m+2; i++) {
		map[i] = (int*)malloc(sizeof(int)*n+2);
	}
	for (i = 0; i < n + 2; i++) {
		map[0][i] = 2;
		map[m + 1][i] = 2;
	}
	for (i = 0; i < m + 2; i++) {
		map[i][0] = 2;
		map[i][n + 1] = 2;
	}
	
	for (i = 1; i < m+1; i++) {
		for (j = 1; j < n+1; j++) {
			scanf("%d", &map[i][j]);
		}
	}
	printf("\n");
	
	printf("\n");
	printf("请输入开始位置和结束位置:\n");
	scanf("%d%d", &startPos.x, &startPos.y);
	scanf("%d%d", &endPos.x, &endPos.y);
	startPos.x += 1;
	startPos.y += 1;
	endPos.x += 1;
	endPos.y += 1;
	queue.rear = 0;
	queue.front = 0;

	startPos.ex = -1;

	queue.data[queue.rear] = startPos;
	queue.rear++;
	nowPos = startPos;
	nowPos.ex = -1;
	map[nowPos.x][nowPos.y] = 2;
	while ((queue.rear != queue.front) && (nowPos.x != endPos.x || nowPos.y != endPos.y)) {
		if (0 == map[nowPos.x + 1][nowPos.y]) {
			temp = nowPos;
			temp.x++;
			temp.ex = queue.front;
			queue.data[queue.rear] = temp;
			queue.rear++;
			map[nowPos.x + 1][nowPos.y] = 2;
		}
		if (0 == map[nowPos.x][nowPos.y + 1]) {
			temp = nowPos;
			temp.y++;
			temp.ex = queue.front;
			queue.data[queue.rear] = temp;
			queue.rear++;
			map[nowPos.x][nowPos.y + 1] = 2;
		}
		if (0 == map[nowPos.x][nowPos.y - 1]) {
			temp = nowPos;
			temp.y--;
			temp.ex = queue.front;
			queue.data[queue.rear] = temp;
			queue.rear++;
			map[nowPos.x][nowPos.y - 1] = 2;
		}

		if (0 == map[nowPos.x - 1][nowPos.y]) {
			temp = nowPos;
			temp.x--;
			temp.ex = queue.front;
			queue.data[queue.rear] = temp;
			queue.rear++;
			map[nowPos.x - 1][nowPos.y] = 2;
		}
		queue.front++;
		nowPos = queue.data[queue.front];

	}
	k = nowPos.ex;
	printf("路径为:\n");
	while (k >= 0) {
		printf("%d,%d\n", nowPos.x-1, nowPos.y-1);
		k = nowPos.ex;
		nowPos = queue.data[k];
	}
	system("pause");
	return 0;
}

```