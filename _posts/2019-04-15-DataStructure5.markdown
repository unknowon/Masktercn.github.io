---
layout: post
title: 数据结构 C语言 链式二叉树
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】  
采用二叉链表作为二叉树的存储结构实现各项功能  
【任务要求】  
(1)	 输入二叉树的先序序列，建立二叉树；  
(2)	  用程序实现二叉树的中序遍历；  
(3)	  编写程序求二叉树的深度；   
【测试数据】  
测试数据：    
（1）输入先序遍历-+a##*b##-c##d##/e##f##；查看其中序遍历、后序遍历和该二叉树的深度。  
（2）输入先序遍历ab#d##ce###;查看其中序遍历、后序遍历和该二叉树的深度。  

```c
#include<stdio.h>
#include<stdlib.h>

int depth = 0;

typedef struct node{
	char ch;
	struct node *lchild, *rchild;
}linkTree;

linkTree *createTree()
{
	linkTree *t;
	char temp;
	scanf("%c", &temp);
	if (temp == '#') {
		t = NULL;
	}
	else {
		t = (linkTree*)malloc(sizeof(linkTree));
		t->ch = temp;
		t->lchild = createTree();
		t->rchild = createTree();
	}
	return t;
}

int preOrder(linkTree *t)
{
	if (t != NULL) {
		printf("%c", t->ch);
		preOrder(t->lchild);
		preOrder(t->rchild);
	}
}

int inOrder(linkTree *t)
{
	if (t != NULL) {
		inOrder(t->lchild);
		printf("%c", t->ch);
		inOrder(t->rchild);
	}
}

int postOrder(linkTree *t)
{
	if (t != NULL) {
		postOrder(t->lchild);
		postOrder(t->rchild);
		printf("%c", t->ch);
	}
}

int treeDeep(linkTree *t, int level)
{
	if (t) {
		if (level > depth) {
			depth = level;
		}
		treeDeep(t->lchild, level + 1);
		treeDeep(t->rchild, level + 1);
	}
}

int main()
{
	linkTree *t = createTree();
	int level = 1;
	printf("\n");
	preOrder(t);
	printf("\n");
	inOrder(t);
	printf("\n");
	postOrder(t);
	printf("\n");
	treeDeep(t, level, &depth);
	printf("depth:%d", depth);
	system("pause");
	return 0;
}
```