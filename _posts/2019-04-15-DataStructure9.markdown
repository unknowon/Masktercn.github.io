---
layout: post
title: 数据结构 C语言 最小生成树 prim kruskal
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】

在n个城市之间建设网络，只需保证连通即可，求最经济的架设方法。

对于图，其生成树中的边也带权，将生成树各边的权值总和称为生成树的权，并将权值最小的生成树称为最小生成树（Minimun Spanning Tree），简称为MST。有两种非常典型的算法：Prim算法和kruskal算法。

【任务要求】

设计程序完成如下功能：对给定的网和起点，用PRIM算法和kruskal算法的基本思想求解出所有的最小生成树。存储结构可自行选择。

【测试数据】

6个顶点，10条边，边及权值如下

0 1 6

0 2 1

0 3 5

1 2 5

1 4 3

2 3 5

2 4 6

2 5 4

3 5 2

4 5 6




```c
#include<stdio.h>
#include<stdlib.h>
#define N 6
#define E 10

typedef struct {
	char name[N][20];
	float arcs[N][N];
}graph;

typedef struct {
	int from, to;
	float w;
	int index;
}edge;

typedef struct {
	int value;
	int aggregate;
}spot;

int prim(graph *g, edge *t)
{
	int i, j, m, v, k;
	float d, min;
	edge temp;
	for (i = 1; i < N; i++) {
		t[i - 1].from = i;
		t[i - 1].to = 0;
		t[i - 1].w = g->arcs[i][0];
	}
	for (i = 0; i < N - 1; i++) {
		min = INT_MAX;
		for (j = i; j < N - 1; j++) {
			if (t[j].w < min) {
				min = t[j].w;
				m = j;
			}
		}
		temp = t[m];
		t[m] = t[i];
		t[i] = temp;
		v = t[i].from;
		for (j = i + 1; j < N - 1; j++) {
			d = g->arcs[v][t[j].from];
			if (d < t[j].w) {
				t[j].w = d;
				t[j].to = v;
			}
		}
	}
	for (k = 0; k < N - 1; k++) {
		printf("%s\t%s\t%.0f\n", g->name[t[k].from], g->name[t[k].to], t[k].w);
	}
}
int kruskal(edge *e, spot *spo)
{
	int i, j;
	int min, n;
	int start, end;
	int ws=0, x;
	edge temp;
	for (i = 0; i < E - 1; i++) {
		min = e[i].w;
		n = i;
		for (j = i + 1; j < E - 1; j++) {
			if (e[j].w < min) {
				min = e[j].w;
				n = j;
			}
		}
		if (n != i) {
			temp = e[i];
			e[i] = e[n];
			e[n] = temp;
		}
	}
	for (i = 0; i < E - 1; i++) {
		start = e[i].from;
		end = e[i].to;
		if (spo[start].aggregate != spo[end].aggregate) {
			x = spo[start].aggregate;
			for (j = 0; j < N; j++) {
				if (x == spo[j].aggregate) {
					spo[j].aggregate = spo[end].aggregate;
				}
			}
			ws += e[i].w;
			e[i].index = 1;
		}
	}
	for (i = 0; i < E; i++) {
		if (e[i].index == 1) {
			printf("%d\t%d\t%.0f\n", e[i].from, e[i].to, e[i].w);
		}
	}
}

int main()
{
	graph g;
	edge t[N - 1];

	spot spo[N];
	edge e[E];
	
	int i, j, m, n;
	float w;
	for (i = 0; i < N; i++) {
		scanf("%s", g.name[i]);
		spo[i].value = g.name[i];
		spo[i].aggregate = i;
	}
	for (i = 0; i < N; i++) {
		for (j = 0; j < N; j++) {
			g.arcs[i][j] = INT_MAX;
		}
	}
	for (i = 0; i < E; i++) {
		scanf("%d%d%f", &m, &n, &w);
		g.arcs[m][n] = w;
		g.arcs[n][m] = w;
		e[i].from = m;
		e[i].to = n;
		e[i].w = w;
		e[i].index = 0;
	}
	printf("prim:\n");
	prim(&g, t);
	printf("kruskal:\n");
	kruskal(e, spo);
	system("pause");
	return 0;
}
```
