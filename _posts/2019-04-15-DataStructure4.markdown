---
layout: post
title: 数据结构 C语言 模式匹配 文件读取
date: 2017-07-03 20:43:23.000000000 +09:00
category: Data structure
---
【问题描述】  
文学研究人员需要统计某篇文章中某些词的出现次数。试写一个实现这一目标的文字统计系统  
【任务要求】  
文章存于一个文本文件中。待统计的词汇集合要一次输入完毕，即统计工作必须在程序的一次运行之后就全部完成。程序的输出结果是每个词的出现次数格式自行设计。  
【测试数据】  
与程序放在同一目录下的记事本文件xxx.txt,内容如下“某日，一个大学老师提问学生，树上有十只鸟，开枪打死一只，还剩几只？学生反问：是无声手枪吗？不是枪声有多大？80-100分贝。在这个城市打鸟犯不犯法？不犯。您确定那只鸟真的被打死了吗？确定。这时，老师已经不耐烦了：“，你告诉我还剩几只鸟就行了，OK？树上的鸟里有没有聋子？没有。有没有被关在笼子里挂在树上的？没有。边上有没有其他的树，树上还有没有其他的鸟？没有。如果有鸟怀孕了，算不算肚子里的小鸟？不算。 打鸟的人眼有没有花？没有花，就十只。老师已经是满头是汗，且下课铃响，但学生继续问：有没有傻得不怕死的鸟？都怕死。会不会一枪打死两只？不会。学生满怀信心地说：，如果您的回答没有骗人“打死的鸟要是挂在树上没有掉下来，那么就剩一只，如果掉下来，就一只不剩。老师当即口吐白沫倒在地上！”  
统计文档中的“学生”出现的次数。  


```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

int count = 0;

typedef struct {
	char str[5000];
	int len;
}string;

int readText(string *s, char *fileName)
{
	FILE *fp;
	char ch;
	long len;
	int i;
	fp = fopen(fileName, "r");
	if (!fp) {
		printf("读取失败\n");
		return 0;
	}
	fseek(fp, 0L, 2);
	len = ftell(fp);
	rewind(fp);
	i = 0; 
	while (i < len) {
		ch = fgetc(fp);
		s->str[i] = ch;
		i++;
	}
	s->len = len;
	s->str[s->len] = '\0';
	fclose(fp);
	return 1;
}

int mspp(string *s1, string *s2, int m)
{
	int i = m - 1, j = 0;
	while (i < s1->len && j < s2->len) {
		if (s1->str[i] == s2->str[j]) {
			i++;
			j++;
			if (j == s2->len) {
				count++;
				return i - s2->len;
			}
		}
		else {
			i = i - j + 1;
			j = 0;
		}
	}
}

int statistics(string *s, string *ss)
{
	int k;
	k = mspp(s, ss, 0);
	while (k < s->len)
	{
		k = mspp(s, ss, k+ss->len);
		if (k == -1)
			break;
	}
}

int main()
{
	string s, ss;
	readText(&s, "Text.txt");
	strcpy(ss.str, "学生");
	ss.len = strlen(ss.str);
	statistics(&s, &ss);
	printf("学生出现的次数是:%d\n", count);
	system("pause");
	return 0;
}
```