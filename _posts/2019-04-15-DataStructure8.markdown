---
layout: post
title: 数据结构 C语言 哈夫曼编码 哈夫曼树 文件操作
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】  

利用哈夫曼编码进行通信可以大大提高信道利用率，缩短信息传输时间，降低传输成本。但是，这要求在发送端通过一个编码系统对待传数据预先编码，在接收端将传来的数据进行译码（复原）。对于双工信道（即可以双向传输信息的信道），每端都需要一个完整的编/译码系统。试为这样的信息收发站写一个哈夫曼码的编/译码系统。  

【任务要求】  

一个完整的系统应具有以下功能：  

1)      I：初始化（Initialization）。从终端读入字符集大小n，以及n个字符和n个权值，建立哈夫曼树，并将它存于文件hfmTree中。  

2)      E：编码（Encoding）。利用已建好的哈夫曼树（如不在内存，则从文件hfmTree中读入），对文件ToBeTran中的正文进行编码，然后将结果存入文件CodeFile中。  

3)      D：译码（Decoding）。利用已建好的哈夫曼树将文件CodeFile中的代码进行译码，结果存入文件TextFile中。

【测试数据】

用下表给出的字符集和频度的实际统计数据建立哈夫曼树，并实现以下报文的编码和译码：“THIS PROGRAM IS MY FAVORITE”。​​
![报文](/assets/images/DataStructure8Img1.png)


```c
#include<stdio.h>
#include<stdlib.h>
#define N 27
typedef struct {
	char ch;
	int weight;
	int lchild, rchild, parents;
}huffmanTree;

typedef struct {
	char bits[N];
	int start;
	char ch;
}huffmanCode;

int createHuffman(huffmanTree tree[], int n)
{
	int i, j;
	int min, cmin;
	int m, cm;
	char ch;

	printf("请输入所有字符（每个字符后面一个回车）：\n");
	for (i = 0; i < n; i++) {
		ch = getchar();
		tree[i].ch = ch;
		getchar();
	}
	printf("ok\n请输入所有权值:\n");
	for (i = 0; i < n; i++) {
		tree[i].lchild = tree[i].rchild = tree[i].parents = -1;
		scanf("%d", &tree[i].weight);
	}
	for (i = n; i < 2 * n - 1; i++) {
		min = cmin = 9999999;
		m = cm = 0;
		for (j = 0; j < i; j++) {
			if (tree[j].parents == -1) {
				if (tree[j].weight < cmin) {
					min = cmin;
					m = cm;
					cmin = tree[j].weight;
					cm = j;
				}
				else if (tree[j].weight < min) {
					min = tree[j].weight;
					m = j;
				}
			}

		}
		tree[i].weight = tree[m].weight + tree[cm].weight;
		tree[i].lchild = m;
		tree[i].rchild = cm;
		tree[i].parents = -1;
		tree[m].parents = i;
		tree[cm].parents = i;
	}
	return 0;
}

int incode(huffmanTree *tree, huffmanCode *code)
{
	int i, parent, child;
	huffmanCode temp;
	for (i = 0; i < N; i++) {
		child = i;
		parent = tree[child].parents;
		temp.start = N;
		while (parent != -1) {
			if (tree[parent].lchild == child) {
				temp.bits[--temp.start] = '0';
			}
			else {
				temp.bits[--temp.start] = '1';
			}
			child = parent;
			parent = tree[child].parents;
		}
		code[i] = temp;
	}
}

int huffcode(huffmanTree *tree, huffmanCode *code)
{
	char c;
	int j, index;
	FILE *fp, *write;
	if ((fp = fopen("ToBeTran.txt", "r")) == NULL) {
		printf("文件读取失败\n");
		return 0;
	}
	if ((write = fopen("CodeFile.txt", "w")) == NULL) {
		printf("文件读取失败\n");
		return 0;
	}

	c = fgetc(fp);
	while (c != '#') {
		if (c == 32) {
			for (j = code[0].start; j <= N - 1; j++) {
				fprintf(write, "%c", code[0].bits[j]);
			}
		}
		else {
			index = c - 'A' + 1;
			for (j = code[index].start; j <= N - 1; j++) {
				fprintf(write, "%c", code[index].bits[j]);
			}
		}
		c = fgetc(fp);
	}
	fprintf(write,"#");
	fclose(fp);
	fclose(write);
	return 0;
}

int decode(huffmanTree *tree, huffmanCode *code)
{
	int i;
	char b;
	FILE *fp, *write;
	if ((fp = fopen("CodeFile.txt", "r")) == NULL) {
		printf("文件读取失败\n");
		return 0;
	}
	if ((write = fopen("TextFile.txt", "w")) == NULL) {
		printf("文件读取失败\n");
		return 0;
	}

	b = fgetc(fp);
	i = 2 * N - 2;
	while (b != '#')
	{
		if (b == '0')
			i = tree[i].lchild;
		else if (b == '1')
			i = tree[i].rchild;
		if (tree[i].lchild == -1)
		{
			fprintf(write, "%c", tree[i].ch);
			i = 2 * N - 2;
		}
		b = fgetc(fp);
	}
	if (tree[i].lchild != -1 && i != 2 * N - 2) {
		printf("电文有错\n");
	}
	fclose(fp);
	fclose(write);
}


int main()
{
	FILE *fp, *du;
	int i, n, j;
	huffmanTree *tree;
	huffmanCode *code;

	printf("请输入个数:\n");
	scanf("%d", &n);
	getchar();
	tree = (huffmanTree*)malloc(sizeof(huffmanTree)*(2 * n - 1));
	code = (huffmanCode*)malloc(sizeof(huffmanCode) * n);
	if ((fp = fopen("hfmTree.txt", "w")) == NULL) {
		printf("文件打开失败\n");
	}

	createHuffman(tree, 27);
	//------------------------------------------------------------------------------------------------------------------------
	//写文件
	for (i = 0; i < n; i++) {
		fprintf(fp,"%c %d %d %d %d\n", tree[i].ch, tree[i].parents, tree[i].lchild, tree[i].rchild, tree[i].weight);
	}
	for (i = n; i < 2 * n - 1; i++) {
		fprintf(fp, "%d %d %d %d\n", tree[i].parents, tree[i].lchild, tree[i].rchild, tree[i].weight);
	}
	fclose(fp);
	/*for (i = 0; i < 2 * n - 1; i++) {
		printf("%c,%d,%d,%d,%d\n", tree[i].ch, tree[i].parents, tree[i].lchild, tree[i].rchild, tree[i].weight);
	}*/
	//------------------------------------------------------------------------------------------------------------------------
	//读树文件
	if ((du = fopen("hfmTree.txt", "r")) == NULL) {
		printf("打开文件失败\n");
	}
	for (i = 0; i < n; i++) {
		fscanf(du, "%d %d %d %d\n", &tree[i].parents, &tree[i].lchild, &tree[i].rchild, &tree[i].weight);
	}
	for (i = n; i < 2 * n - 1; i++) {
		fscanf(du, "%d %d %d %d\n", &tree[i].parents, &tree[i].lchild, &tree[i].rchild, &tree[i].weight);
	}
	fclose(du);
	//------------------------------------------------------------------------------------------------------------------------
	/*for (i = 0; i < 2 * n - 1; i++) {
		printf("%c,%d,%d,%d,%d\n", tree[i].ch, tree[i].parents, tree[i].lchild, tree[i].rchild, tree[i].weight);
	}*/

	incode(tree, code);
	for (i = 0; i < N; i++) {
		printf("%c : ", tree[i].ch);
		for (j = code[i].start; j <= N - 1; j++) {
			printf("%c", code[i].bits[j]);
		}
		printf("\n");
	}
	huffcode(tree, code);
	decode(tree, code);
	fclose(fp);
	system("pause");
	return 0;
}
```


